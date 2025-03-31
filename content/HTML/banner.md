---
title: "Banner: 語義化與視覺化的差別"
date: 2025-03-31
draft: false
url: "/html/banner"
---

# 前言:

哈摟，各位朋友好，今天想說點 HTML，為什麼想說？我在探索 aria 相關的屬性，發現一個有趣的事 就是 role=banner，我心想 banner 通常不是都放在 header 下方？他也有語義？banner 其實有分語義與視覺，今天來談談在還沒有 HTML5 之前是如何定義 header，語義上的 banner 又是什麼回事？讓我們看下去吧

## banner 語義上是 header

1. 語義上的橫幅

```html
<!--  before HTML5 -->
<div role="banner">
	<div class="logo">
		<h1>網站名稱</h1>
	</div>
	<nav>
		<ul>
			<li><a href="#">首頁</a></li>
			<li><a href="#">產品</a></li>
			<li><a href="#">聯繫我們</a></li>
		</ul>
	</nav>
</div>

<!--  after HTML5 -->
<header>
	<div class="logo">
		<h1>網站名稱</h1>
	</div>
	<nav>
		<ul>
			<li><a href="#">首頁</a></li>
			<li><a href="#">產品</a></li>
			<li><a href="#">聯繫我們</a></li>
		</ul>
	</nav>
</header>
```

2. 視覺上的橫幅:

```html
<!-- 視覺上的橫幅 (例如廣告或通知條) -->
<section class="visual-banner">
	<p>限時優惠：全站商品 8 折，快來搶購！</p>
</section>
```

# 總結:

banner 在 HTML 中有兩種不同含義。語義層面的 banner 指網站頂部主要標題區域，通常包含網站標誌、名稱和主導航;視覺層面的 banner 則是指網頁中的橫幅廣告或通知條，無特定語義標籤。
理解這兩種 banner 的區別有助於建立符合標準且具有良好可訪問性的網頁。
