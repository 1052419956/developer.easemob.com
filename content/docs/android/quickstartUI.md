---
title: 环信
sidebar: androidsidebar
secondnavandroid: true
---

#### 联系人变化listener：见MainActivity.java #### 
 
<pre class="hll"><code class="language-java">
    private class MyContactListener implements EMContactListener{

		@Override
		public void onContactAdded(List<String> usernameList) {
			
			
		}

		@Override
		public void onContactDeleted(List<String> usernameList) {
			
		} 
	}
</code></pre>


#### 监听连接状态和账号多处登录被迫下线：见MainActivity.java ####
 
<pre class="hll"><code class="language-java">
    private class MyConnectionListener implements ConnectionListener{
		@Override
		public void onConnected() {
		}
	}
</code></pre>



