---
title: Initialize Audio Kit SDK and Play Audio
description: 15
---

<p><strong>1. Locate following line in AudioActivity.kt </strong></p>
<pre><div id="copy-button10" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Initialize managers here<span class="pln">
</span></code></pre>
<p><strong>2. Initialize Audio Kit managers</strong></p>
<pre><div id="copy-button11" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>
val hwAudioPlayerConfig = HwAudioPlayerConfig(context)
HwAudioManagerFactory.createHwAudioManager(hwAudioPlayerConfig, object : HwAudioConfigCallBack {
    override fun onSuccess(hwAudioManager: HwAudioManager) {
        try {
            mHwAudioManager = hwAudioManager
            mHwAudioPlayerManager = mHwAudioManager!!.playerManager
            mHwAudioConfigManager = mHwAudioManager!!.configManager
            mHwAudioQueueManager = mHwAudioManager!!.queueManager
            mHwAudioQueueManager!!.addPlayItemList(playList, 0)
            if (playList != null) {
                doListenersAndNotifications()
            } else {
                Log.e("TAG", "playlist null")
            }
        } catch (e: Exception) {
            Log.e("TAG", "player init fail", e)
        }
    }
    override fun onError(errorCode: Int) {
        Log.e("TAG", "init err:$errorCode")
    }
})<span class="pln">
</span></code></pre>

<p><strong>3. Locate following line in AudioActivity.kt</strong></p>
<pre><div id="copy-button12" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Attach listeners to the manager<span class="pln">
</span></code></pre>
<p><strong>4.Attach listeners to the manager to control playbacks</strong></p>
<pre><div id="copy-button13" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>addListenerToManager(mPlayListener)<span class="pln">
</span></code></pre>

<aside class="special">
	<p><strong>Note: AsyncTask is currently deprecated, please use other threading methods if you care about modernity in your application.</strong></p>
</aside>

<p><strong>5. Locate following line in AudioActivity.kt</strong></p>
<pre><div id="copy-button19" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Implement onSongChange method of the listener<span class="pln">
</span></code></pre>
<p><strong>6. Implement the onSongChange method of the listener</strong></p>
<pre><div id="copy-button20" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>
if(hwAudioPlayItem != null)
    setSongDetails(hwAudioPlayItem) //just update the song details everytime a song changes
if (mHwAudioPlayerManager!!.offsetTime != -1L && mHwAudioPlayerManager!!.duration != -1L) 
    updateSeekBar(mHwAudioPlayerManager!!.offsetTime, mHwAudioPlayerManager!!.duration) //also update seekbar if needed<span class="pln">
</span></code></pre>

<p><strong>7. Locate following line in AudioActivity.kt</strong></p>
<pre><div id="copy-button21" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Implement onPlayProgress method of the listener<span class="pln">
</span></code></pre>
<p><strong>8. Implement onPlayProgress method of the listener</strong></p>
<pre><div id="copy-button22" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>updateSeekBar(currentPosition, duration) //just update the seekbar as this method is called everytime the song progresses<span class="pln">
</span></code></pre>

<p><strong>9. Locate following line in AudioActivity.kt</strong></p>
<pre><div id="copy-button23" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Implement the song progress on Audio Kit manager<span class="pln">
</span></code></pre>
<p><strong>10. Implement the song progress on Audio Kit manager</strong></p>
<pre><div id="copy-button24" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>
if (mHwAudioPlayerManager != null && fromUser) {
    mHwAudioPlayerManager!!.seekTo(progress * 1000)
}</span></code></pre>


--left here---
<p><strong>11. Locate following line in AudioActivity.kt </strong></p>
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
<p><strong>13. Locate following line in AudioActivity.kt</strong></p>
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
<p><strong>15. Locate following line in AudioActivity.kt</strong></p>
<pre><div id="copy-button29" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Implement ReadyListener of WisePlayer<span class="pln">
</span></code></pre>
<p><strong>16. Implement ReadyListener as right after the .start() method, execution will continue from here</strong></p>
<pre><div id="copy-button30" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>  startPlaying()
  runOnUiThread { updatePlayView(player) }<span class="pln">
</span></code></pre>
<p><strong>17. Locate following line in AudioActivity.kt</strong></p>
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
