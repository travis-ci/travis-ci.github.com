---
title: Encryption keys
layout: en
permalink: encryption-keys/
---

Travis generates a pair of private and public RSA keys which can be used
to encrypt information which you will want to put into the `.travis.yml` file and
still keep it private. Currently we allow encryption of
[environment variables](/docs/user/build-configuration/#Secure-environment-variables), notification settings, and deploy api keys.

## Encrypt a Value

You can encrypt a value right here in your browser using [JSEncrypt](https://github.com/travist/jsencrypt).
Neither the unencrypted nor the encrypted value will be sent over the network.

<script src="/jsencrypt.min.js"></script>

<p>
  <label for="repo">repository:</label>
  <input id="repo" pattern="[^\/\s]+\/[^\/\s]+" placeholder="[owner]/[repo]" required="required" type="text">
  <label for="value">value:</label>
  <input id="value" type="text" id="value" placeholder="the secret value" maxlength="128">
  <select id="api">
    <option value="https://api.travis-ci.org" selected>travis-ci.org</option>
    <option value="https://api.travis-ci.com">travis-ci.com</option>
  </select>
  <button id="encrypt">Encrypt</button>
  <div id="output"></div>
</p>

<script>
$("#encrypt").bind("click", function() {
  var headers = { Accept: 'application/json; version=2' };
  var output  = $('#output');
  var api     = $('#api').val();
  var repo    = $("#repo").val();
  var value   = $("#value").val();

  function generate() { fetch_key(encrypt) }

  function encrypt(result) {
    if(result.key) {
      crypt = new JSEncrypt();
      crypt.setPublicKey(result.key);
      pre    = $("<pre></pre>")
      pre.text('secure: ' + crypt.encrypt(value) + '');
      output.html('<p>Place the following in your <em>.travis.yml</em> instead of the unencrypted value:</p>');
      output.append(pre);
    } else {
      output.html("<b>Broken payload from Travis API</b>");
    }
  }

  function fetch_key(callback) {
    output.html("<em>loading…</em>");
    $.ajax({ url: api + '/repos/' + repo + "/key", headers: headers, success: callback,
      error: function() { output.html("<b>Failed to retrieve key, does the repo exist?</b>") }});
  }

  function authenticate(callback) {
    output.html("<em>authenticating…</em>");
    $('<iframe id="auth-frame" />').hide().appendTo('body').attr('src', api + "/auth/post_message?origin=" + window.location.href);
    window.addEventListener('message', function(e) {
      if(e.origin === api) {
        if(e.data.token) {
          headers['Authorization'] = 'token ' + e.data.token;
          callback()
        } else if(e.data === 'redirect') {
          window.location = api + '/auth/handshake?redirect_uri=' + window.location.href;
        } else {
          output.html("<b>Authentication failed!</b>");
        }
      }
    });
  }

  api === 'https://api.travis-ci.com' ? authenticate(generate) : generate();
})
</script>

## CLI Usage

The easiest way to encrypt something with the public key is to use Travis CLI.
This tool is written in Ruby and published as a gem. First, you need to install
the gem:

    gem install travis

Then, you can use `encrypt` command to encrypt data (This example assumes you are running the command in your project directory. If not, add `-r owner/project`):

    travis encrypt "something to encrypt"

This will output a string looking something like:

    secure: ".... encrypted data ...."

Now you can place it in the `.travis.yml` file. You can read more about
[secure environment variables](/docs/user/build-configuration/#Secure-environment-variables)
or [notifications](/docs/user/notifications).

## Fetching the public key for your repository

You can fetch the public key with Travis API, using `/repos/:owner/:name/key` or
`/repos/:id/key` endpoints, for example:

    https://api.travis-ci.org/repos/travis-ci/travis-ci/key

You can also use the `travis` tool for retrieving said key:

    travis pubkey

Or, if you're not in your project directory:

    travis pubkey -r owner/project
