---
title: Create Wiseplayer and Play Video
description: 15
---

<p><strong>1. Locate following line to create the Wise Player Factory instance in WisePlayerInit Object.</strong></p>
<pre><div id="copy-button10" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Initializing of Wise Player Factory<span class="pln">
</span></code></pre>
<p><strong>2. Create the Wise Player Factory instance in a class that extends Application</strong></p>
<pre><div id="copy-button11" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  // Pass the device ID to the setDeviceId method.
  val factoryOptions = WisePlayerFactoryOptions.Builder().setDeviceId("xxx").build()
  // In the multi-process scenario, the onCreate method in Application is called multiple times.
  // The app needs to call the WisePlayerFactory.initFactory() API in the onCreate method of the app process (named "app package name") 
  // and WisePlayer process (named "app package name:player")

  WisePlayerFactory.initFactory(this, factoryOptions, object : InitFactoryCallback {
      override fun onSuccess(wisePlayerFactory: WisePlayerFactory) {
          Log.d(TAG, "onSuccess wisePlayerFactory:$wisePlayerFactory")
          Companion.wisePlayerFactory = wisePlayerFactory
      }
      override fun onFailure(errorCode: Int, msg: String) {
          Log.e(TAG, "onFailure errorcode:$errorCode reason:$msg")
      }
  })<span class="pln">
</span></code></pre>
<p>Description of <strong>Wise Player Factory</strong> is as following:<br></p>
<table style="width: 100%;table-layout: fixed;">
	<tbody><tr></tr>
	<tr><td colspan="1" rowspan="1"><p>Parameter</p>
	</td><td colspan="1" rowspan="1"><p>Type:</p>
	</td><td colspan="1" rowspan="1"><p>Mandatory or Not</p>
	</td><td colspan="1" rowspan="1"><p>Description</p>
	</td></tr>
	<tr><td colspan="1" rowspan="1"><p>context</p>
	</td><td colspan="1" rowspan="1"><p>Context</p>
	</td><td colspan="1" rowspan="1"><p>M</p>
	</td><td colspan="1" rowspan="1"><p>Android context object, which is not set to null.</p>
	</td></tr>
	<tr><td colspan="1" rowspan="1"><p>options</p>
	</td><td colspan="1" rowspan="1"><p>Integer</p>
	</td><td colspan="1" rowspan="1"><p>M</p>
	</td><td colspan="1" rowspan="1"><p>Instance of the WisePlayer factory class initialization option <a href="https://developer.huawei.com/consumer/en/doc/HMSCore-References-V5/wpf-options-0000001050439397-V5" target="_blank">WisePlayerFactoryOptions</a></p>
	</td></tr>
	<tr><td colspan="1" rowspan="1"><p>callback</p>
	</td><td colspan="1" rowspan="1"><p>Object</p>
	</td><td colspan="1" rowspan="1"><p>M</p>
	</td><td colspan="1" rowspan="1"><p>Instance of the <a href="https://developer.huawei.com/consumer/en/doc/HMSCore-References-V5/init-factory-callback-0000001050199187-V5" target="_blank">InitFactoryCallback API</a> for initializing the WisePlayer factory class.</p>
	</td></tr>
</tbody></table>
<p><strong>3. Locate following line in PlayActivity.kt</strong></p>
<pre><div id="copy-button12" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Initialize the Wise Player Factory instance<span class="pln">
</span></code></pre>
<p><strong>4.Initialize the Wise Player Factory instance and set the listeners</strong></p>
<pre><div id="copy-button13" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  //You should call this function in the onCreate() function of the activity.
  if (wisePlayerFactory != null) {
      player = wisePlayerFactory!!.createWisePlayer()
      if(player != null){
          //*setting some unchanging parameters beforehand
          player?.let { player!!.setReadyListener(myReadyListener)}
          player!!.setPlayEndListener(myPlayEndListener)
          player!!.setLoadingListener(myLoadingListener)
          player!!.setErrorListener(myErrorListener)
          player!!.setEventListener(myEventListener)
          player!!.setSeekEndListener(mySeekEndListener)
          player!!.setResolutionUpdatedListener(myResolutionUpdatedListener)
          player!!.setVideoType(PlayerConstants.PlayMode.PLAY_MODE_NORMAL) //or 0
          player!!.setBookmark(10000)
          player!!.setCycleMode(PlayerConstants.CycleMode.MODE_CYCLE) //or 1
          player!!.setPlayMode(0)
          //*ready and start will be made upon onClick.
      }
  }<span class="pln">
</span></code></pre>
<aside class="special">
	<p><strong>Note: Frame Layout is mandatory for SurfaceView to display videos, otherwise some problems with video playback may occur.</strong></p>
</aside>
<br><img style="width: 400.00px" src="https://raw.githubusercontent.com/bengongon97/VideoPlayerWithVideoKit/master/assets/frameLayoutForSurfaceView.png" onclick="imageclick(src)">
<p><strong>5. Locate following line in PlayActivity.kt</strong></p>
<pre><div id="copy-button19" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Implement surfaceCreated method<span class="pln">
</span></code></pre>
<p><strong>6. Implement the method for SurfaceView</strong></p>
<pre><div id="copy-button20" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  if (player != null) {
      player!!.setView(surfaceView)
      // To resume WisePlayer when you bring your app to the foreground, call the resume API. You can determine whether the playback automatically starts after your app is brought to the foreground by passing a parameter.
      player!!.resume(PlayerConstants.ResumeType.KEEP)
  }<span class="pln">
</span></code></pre>
<p><strong>7. Locate following line in PlayActivity.kt</strong></p>
<pre><div id="copy-button21" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Set seekbar listener onStop method<span class="pln">
</span></code></pre>
<p><strong>8. Implement the stop part of Seekbar Listener</strong></p>
<pre><div id="copy-button22" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  if (player != null && hasVideoEverStarted && dialogUtil.vodRetriever()) {
      showBufferingView()
      player!!.seek(seekBar.progress)
      updatePlayProgressView(seekBar.progress, player!!.bufferTime)
  }<span class="pln">
</span></code></pre>
<p><strong>9. Locate following line in PlayActivity.kt</strong></p>
<pre><div id="copy-button23" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Implement updatePlayProgressView<span class="pln">
</span></code></pre>
<p><strong>10. Write the code to update the progress</strong></p>
<pre><div id="copy-button24" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  seekBar!!.progress = progress
  seekBar!!.secondaryProgress = bufferPosition
  seekBar!!.incrementSecondaryProgressBy(bufferPosition - progress)
  progressTextView!!.text = formatLongToTimeStr(progress)<span class="pln">
</span></code></pre>
<p><strong>11. Locate following line in PlayActivity.kt </strong></p>
<pre><div id="copy-button25" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Implement the onItemClick method for video list<span class="pln">
</span></code></pre>
<p><strong>12. Implement the onItemClick method for RecyclerView</strong></p>
<pre><div id="copy-button26" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  currentPlayItem = myVideoList!![position]
  //to ensure that we have the instance because normally getting the instance takes time
  //if cannot be acquired above, it will be acquired here
  while (player == null) {
      initPlayer()
      if (player != null) break
  }
  hasVideoEverStarted = true
  if (player!!.isPlaying) {
      player!!.stop()
      isReallyPlaying = false
  }
  player!!.reset()
  player!!.playSpeed = 1.0f
  playSpeedTextView!!.setText(R.string.onePointZeroText)
  val url = currentPlayItem!!.sources
  player!!.setPlayUrl(url)
  player!!.setView(surfaceView)
  if (!dialogUtil.vodRetriever()) {
      player!!.setVideoType(1)
  }
  showBufferingView()
  player!!.ready()<span class="pln">
</span></code></pre>
<p><strong>13. Locate following line in PlayActivity.kt</strong></p>
<pre><div id="copy-button27" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Implement UI thread to keep track of the progress in seekbar<span class="pln">
</span></code></pre>
<p><strong>14. Implement UI thread runnable to update the progress calculations regularly</strong></p>
<pre><div id="copy-button28" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  runOnUiThread(object : Runnable {
      override fun run() {
          if (player != null && isReallyPlaying) {
              updatePlayProgressView(player!!.currentTime, player!!.bufferTime)
          }
          mHandler.postDelayed(this, 1000) //1 second
      }
  })<span class="pln">
</span></code></pre>
<p><strong>15. Locate following line in Play Activity.</strong></p>
<pre><div id="copy-button29" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Implement ReadyListener of WisePlayer<span class="pln">
</span></code></pre>
<p><strong>16. Implement ReadyListener as right after the .start() method, execution will continue from here</strong></p>
<pre><div id="copy-button30" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  startPlaying()
  runOnUiThread { updatePlayView(player) }<span class="pln">
</span></code></pre>
<p><strong>17. Locate following line in Play Activity.</strong></p>
<pre><div id="copy-button31" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Release WisePlayer and remove callbacks<span class="pln">
</span></code></pre>
<p><strong>18. Release Wise Player and listeners in onDestroy() of PlayActivity.kt</strong></p>
<pre><div id="copy-button32" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  player!!.stop()
  player!!.release()
  player = null
  hasVideoEverStarted = false
  isReallyPlaying = false
  removeMyCallbacks()<span class="pln">
</span></code></pre>
