#### 1. 實作一個購物車功能，要用前端實作，並且不同分頁可以共享，要如何設計？
用 cookie 來儲存購物車的內容，若是需要不同分頁即是更新，可能需要另外處理。
一個方法是定時刷新元件，去取得最新的 cookie。其他不太確定。
#### 2. 有一個直播網站，假設有一千個人同時在聊天室傳送訊息，小明是第一千個，你要如何處理，讓小明不會感受到一千個訊息的延遲？
不等小明的訊息新增至資料庫中，直接顯示在小明的畫面上。並且於獲取聊天室訊息時不要重複顯示小明的訊息。

可以在前端額外去將小明自己的訊息給篩選掉不顯示。或者在後端直接不回傳小明的訊息。
#### 3. 如果有一個網站的登入只能用 get，要怎麼設計讓他比較安全？
首先要對訊息經過加密(RSA 之類公鑰加密)，不過這樣還是無法阻止透過 url 取得加密過後的驗證資料直接拿來用的問題。

因此可以搭配 token，並且限制時間，這樣至少過一段時間後這個加密的驗證資料就失效了。

#### 4. 請問 HTTP/2 是什麼？
對原本 HTTP 1.1 進行一些傳輸速度的優化，包含：
1. 標頭壓縮與編碼: 藉由 Haffman 演算法壓縮 HEAD 來增加傳輸速度
1. 流程下載控制與優先級
1. 不強制採用加密傳輸: 不強制使用 https，但更容易實現 TLS

#### 5. 請解釋一下 CDN?
將檔案分散在不同 server 上，讓user 可以根據自己的位置連接到最順暢的 server 取得檔案。
因為這個做法也達到異地備援的功能 增加可靠性
#### 6. TCP 三次握手(Three-way Handshake)
第一次由客戶端發送訊息給 Server 端他想要建立連線

第二次由 Server 端發送訊息給客戶端他可以建立連線

第三次由客戶端發送訊息給 Server 端他要開始傳輸資料了

如果只有一次握手，客戶端無法確認 Server 是否正常

如果只有兩次握手，Server 無法確認客戶端是否還正常

SYN 攻擊：發送大量第一次握手的要求，並且不回復第三次握手，用來癱瘓 Server

第三次握手如果失敗的話，Server 會根據設定的重試重傳，如果次數用盡就會取消此次連線，此時如果又收到客戶端的第三次握手請求會回傳 RTS 要求，要求客戶端重新握手

#### 7. TCP 四次揮手(Four-way Handshake)
第一次由客戶端發送訊息給 Server 端他想要中斷連線

第二次由 Server 端發送訊息給客戶端他收到中斷連線的請求

第三次由 Server 端發送訊息給客戶端他準備好中斷連線

第四次由客戶端發送訊息給 Server 端他要中斷連線了，客戶端發出此訊息後等待 2 MSL(Maximum Segment Lifetime) 就會中斷，Server 收到此訊息後就會中斷連線

如果第四次揮手失敗，Server 端會重傳第三次揮手訊息

等待 2 MSL 的意義：確保第四次揮手的訊息有傳給 Server 端，並確保沒有 Server 重傳訊息回來

#### 8. 對稱加密與非對稱加密

對稱性加密：使用共同的 key 加密，key 在傳輸過程中被攔截很容易就被破解(AES)

非對稱加密：有 public key 和 private key 使用 public key 加密 private key 解密(反過來亦可)(RSA)

數位簽章：用來確定為本人傳送，將訊息本身 hash 之後再用他的 private key 加密，接收者收到後先用 public key 解出此 hash 再自己計算收到的訊息 hash 比對是否相同，以此證明

#### 9. http cache

http 1.1 : 新 header Cache-Control: max-age=30

max-age=30 表示這個 request 的有效期限為 30 秒

max-age=0 ≒ no-cache 為每次發送 request 的時候去確認是否有新檔案，如果沒有則繼續使用 cache 資料

no-store 為完全不使用 cache 每次 request 都重新取得檔案

#### 10. browser render process

parse HTML 產生 DOM tree ，遇到 \<script\> 會停下並開始下載、解析執行內容，之後開始 parse CSS 產生 render tree 再實際繪製畫面

#### 11. 優化 SEO 

找出關鍵字，優化網頁效能，適當的網域名稱，使用 https

#### 12. https

使用 SSL/TLS 加密

流程:

1. Hello - 客戶端向伺服器端發起請求，提供支援的加密方法種類

2. 憑證交換 - 伺服器提供憑證供客戶端驗證

    驗證方法：1. 此憑證為可信任的機構發行 2. 此憑證是被可信任的機構所認可(使用數位簽章的方式認證)
3. 認證成功之後做 key 的交換，之後以對稱方式加密通訊

#### 13. CSRF(Cross-site request forgery)

在不同的 domain 底下卻發出使用者本人的 requst

#### 14. XSS (Cross-Site Scripting)

Stored XSS (儲存型) : 資料庫儲存的內容包含可執行的 js 語法

Reflected XSS (反射型) : 網頁上的輸入包含可執行的 js 語法

DOM-Based XSS : JS 執行中可以被插入 js 語法

防範方法 : 

Stored XSS, Reflected XSS 過濾特殊字元

DOM-Based : 避免使用如 dom.innerHTML 之類的會將結果以 HTML 的方式顯示的寫法

