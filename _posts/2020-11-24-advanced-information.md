---
title: Advanced Features
description: 10
---

<ol type="1">
  <li><h3>Advanced Playback Features:</h3></li>
  <p>Audio Kit provides advanced playback capabilities such as shuffle and loop, apart from the basic features like play, pause and next.</p>
  
<p><strong>1. Locate following line in AudioActivity.kt</strong></p>
<pre><div id="copy-button33" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Implement advanced playback controls<span class="pln">
</span></code></pre>
<p><strong>2. Implement advanced playback controls with four modes</strong></p>
<pre><div id="copy-button34" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>
/*
Playback mode.
    0: sequential playback
    1: shuffling songs
    2: repeating a playlist
    3: repeating a song
*/
val shuffleDrawable = getDrawable(R.drawable.menu_shuffle_normal)
val orderDrawable = getDrawable(R.drawable.menu_order_normal)
val loopItself = getDrawable(R.drawable.menu_loop_one_normal)
val loopPlaylist = getDrawable(R.drawable.menu_loop_normal)
binding!!.shuffleButtonImageView.setOnClickListener {
    if (mHwAudioPlayerManager != null) {
        if (binding!!.shuffleButtonImageView.drawable.constantState == shuffleDrawable!!.constantState) {
            mHwAudioPlayerManager!!.playMode = 0
            binding!!.shuffleButtonImageView.setImageDrawable(getDrawable(R.drawable.menu_order_normal))
            Toast.makeText(this@AudioActivity, "Normal order", Toast.LENGTH_SHORT).show()
        } else if (binding!!.shuffleButtonImageView.drawable.constantState == orderDrawable!!.constantState) {
            mHwAudioPlayerManager!!.playMode = 1
            binding!!.shuffleButtonImageView.setImageDrawable(getDrawable(R.drawable.menu_shuffle_normal))
            Toast.makeText(this@AudioActivity, "Shuffle songs", Toast.LENGTH_SHORT).show()
        }
    }
}
binding!!.loopButtonImageView.setOnClickListener {
    if (mHwAudioPlayerManager != null) {
        if (binding!!.loopButtonImageView.drawable.constantState == loopItself!!.constantState) {
            mHwAudioPlayerManager!!.playMode = 2
            binding!!.loopButtonImageView.setImageDrawable(getDrawable(R.drawable.menu_loop_normal))
            Toast.makeText(this@AudioActivity, "Loop playlist", Toast.LENGTH_SHORT).show()
        } else if (binding!!.loopButtonImageView.drawable.constantState == loopPlaylist!!.constantState) {
            mHwAudioPlayerManager!!.playMode = 3
            binding!!.loopButtonImageView.setImageDrawable(getDrawable(R.drawable.menu_loop_one_normal))
            Toast.makeText(this@AudioActivity, "Loop the song", Toast.LENGTH_SHORT).show()
        }
    }
}<span class="pln">
</span></code></pre>

 <li><h3>Close button for notification bar</h3></li>
<p>The notification bar is implemented in the sample app code. A very important part of it is the close button in the design - it should be functional as users may want to close the playback using that button. Since it is located on the notification bar, special stop code will not work and some intents are needed as shown below.</p>
<p><strong>1. Locate following line in MyService.kt</strong></p>
<pre><div id="copy-button35" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Implement close intent for notification bar<span class="pln">
</span></code></pre>
<p><strong>2. Implement close intent for notification bar</strong></p>
<pre><div id="copy-button36" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>
val action = intent.action
if ("com.huawei.hms.mediacenter.cancel_notification" == action) {
    Log.i("TAG", "onReceive----->cancelNotification")
    mHwAudioPlayerManager!!.stop()
}<span class="pln">
</span></code></pre>

 <li><h3>Service Implementation</h3></li>
<p>Sample app contains MyService.kt that gets a very crucial job done. It kills the app once the user destroys the app (slides up in the recent apps view). Normally, without this service, your onDestroy() implementation will not be effective even if call playerManager.stop() method.</p>
<p><strong>1. Locate following line in AudioActivity.kt</strong></p>
<pre><div id="copy-button35" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> //TODO: Implement kill process<span class="pln">
</span></code></pre>
<p><strong>2. Implement kill process line to kill the app after the service is also killed</strong></p>
<pre><div id="copy-button36" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code>
Process.killProcess(Process.myPid())<span class="pln">
</span></code></pre>
</ol>
