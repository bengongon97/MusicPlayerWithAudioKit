---
title: Preloader Setup and Usage
description: 10
---

<ol type="1">
  <li><h3>Preloader Setup:</h3></li>
  <p>Preloader allows users to load their videos in advance to be able to play them later on, especially when the internet connection is problematic. It downloads the video data to local storage and therefore, users need to give explicit consent for the storage permission.</p>
  
<p><strong>1. Locate following line in PlayActivity.kt</strong></p>
<pre><div id="copy-button33" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Request storage permission<span class="pln">
</span></code></pre>
<p><strong>2. Implement the user request code</strong></p>
<pre><div id="copy-button34" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  return (ContextCompat.checkSelfPermission(this, Manifest.permission.WRITE_EXTERNAL_STORAGE) == PackageManager.PERMISSION_GRANTED
                && ContextCompat.checkSelfPermission(this, Manifest.permission.READ_EXTERNAL_STORAGE) == PackageManager.PERMISSION_GRANTED)
                //please also check the follow-up code in the actual app<span class="pln">
</span></code></pre>

 <li><h3>Preloader Usage:</h3></li>
<p>Preloader has quite some code to be implemented to be able to work fully. It has "Add, Pause, Resume, Delete Tasks, Delete Data" sub-functionalities. Do the below steps and then check the detailed code in the app.</p>
<p><strong>1. Locate following line in PlayActivity.kt</strong></p>
<pre><div id="copy-button35" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Implement Preloader dialog<span class="pln">
</span></code></pre>
<p><strong>2. Implement dialog call with checks of user permission</strong></p>
<pre><div id="copy-button36" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  if (checkReadPermissionBoolean()) {
      dialogUtil.addPreloadTaskDialog(this)
  } else {
      requestPermission()
  }<span class="pln">
</span></code></pre>
</ol>
