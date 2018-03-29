    ## Đâu là những lỗi phổ biến hay mắc phải của những nhà phát triển ứng dụng trong quá trình phát triển cơ sở dữ liệu.
    
    _source https://stackoverflow.com/questions/621884/database-development-mistakes-made-by-application-developers_
    
    **1. Không sử dụng các chỉ mục thích hợp**
    
    Đây là 1 vấn đề tương đối đơn giản những vẫn xảy ra hàng ngày. Các khóa ngoại nên có các chỉ mục. Nếu bạn đang sử dụng 1 trường trong mệnh đề WHERE bạn nên (có lẽ) có chỉ mục trên nó. Các chỉ mục thường nên gồm nhiều cột dựa trên câu truy vấn bạn thực hiện.
    
    **2. Không sử dụng các ràng buộc tham chiếu**
    
    Cơ sở dữ liệu của bạn có thể thay đổi nhưng nếu nó hỗ trợ ràng buộc tham chiếu-- tức là tất cả các khóa ngoại được đảm bảo cho 1 thực thể tổn tại -- bạn nên sử dụng nó.
    
    Rất hay thấy lỗi như thế này trên cơ sở dữ liệu MySQL. Tôi không nghĩ rằng MyISAM hỗ trợ nó. Nhưng InnoDB thì có. Bạn sẽ thấy những người sử dụng MyISAM hay những cơ sở dữ liệu khác hỗ trợ InnoDB nhưng không sử dụng nó
    
    Thêm nữa:
    
    - [Các ràng buộc như NOT NULL hay Khóa Ngoại quan trọng như thế nào khi ta luôn luôn kiểm soát đầu vào cơ sở dữ liệu với php](https://stackoverflow.com/questions/382309/how-important-are-constraints-like-not-null-and-foreign-key-if-ill-always-contr)
    - [Khóa ngoại có thực sự cần thiết trong thiết kế cơ sở dữ liệu?](https://stackoverflow.com/questions/18717/are-foreign-keys-really-necessary-in-a-database-design)
    - [Khóa ngoại có thực sự cần thiết trong thiết kế cơ sở dữ liệu?](http://www.diovo.com/2008/08/are-foreign-keys-really-necessary-in-a-database-design/)
    
    **3. Sử dụng khóa tự nhiên thay vì (kỹ thuật) khóa chính đại diện*
    
    Khóa tự nhiên là các khóa dựa trên các dữ liệu có nghĩa bên ngoài mà (có vẻ) độc nhất. 1 vài ví dụ phổ biến là các code sản xuất, mã bang 2-chữ-cái (US), số bảo mật xã hội và vân vân. Khóa đại diện hay kĩ thuật khóa chính tuyệt đối không có ý nghĩa bên ngoài hệ thống. Nó được phát minh hoàn toàn là để định nghĩa các thực thể và là các trường tự tăng  (SQL Server. MySQL, khác nưã) hoặc chuỗi(đặc biệt nhất là (Oracle) 
    
    Theo ý kiến của tôi, bạn nên **luôn luôn** sử dụng khóa đại diện. Vấn đề này tới từ những câu hỏi sau đây
    
    - [Bạn thích khóa chính của bạn như thế nào?](https://stackoverflow.com/questions/404040/how-do-you-like-your-primary-keys)
    - [Cách tốt nhất để thực hành khóa chính trên các bảng?](https://stackoverflow.com/questions/337503/whats-the-best-practice-for-primary-keys-in-tables)
    - [Nên dùng khóa chính loại nào trong trường hợp này.](https://stackoverflow.com/questions/506164/which-format-of-primary-key-would-you-use-in-this-situation)
    - [Khóa đại diện và khóa thường/kinh tế](https://stackoverflow.com/questions/63090/surrogate-vs-natural-business-keys)
    - [Chúng ta có nên có 1 trường khóa chính?](https://stackoverflow.com/questions/166750/should-i-have-a-dedicated-primary-key-field)
    
    Đây phần nào là 1 chủ đề gây tranh cãi khi không được chấp nhận rộng rãi. Trong khi bạn cố gắng tìm ra 1 người nghĩ rằng khóa thường trong 1 vài trường hợp thì ok, bạn sẽ không thấy bất kỳ sự chỉ trích nào dành cho khóa đại diện cũng như cho rằng nó không cần thiết, Đây là 1 nhược điểm nhỏ nếu bạn hỏi tôi
    
    
    Nhớ rằng, thậm chí [ các quốc gia không còn tồn tại][countries can cease to exist](http://en.wikipedia.org/wiki/ISO_3166-1) (ví dụ, Nam Tư)
    **4. Viết các câu truy vấn được yêu cầu Để DISCTINCE hoạt động**
    
    Bạn sẽ thường thấy nó trong các câu truy vấn khởi tạo ORM. Nhìn log đầu ra trong Hibernate và bạn sẽ thấy tất cả các câu truy vấn bắt đầu với:
    
    SELECT DISTINCT ...
    
    Sẽ mất 1 lục để đảm bảo bạn không tạo ra các dòng bị trùng và dẫn đến các đối tượng bị trùng. Bạn sẽ thỉnh thoảng thấy vài người làm như vậy. Nếu bạn thấy điều này quá nhiều thì thực sự đáng báo động đấy.Không phải do DISTINCE tệ háy không có các ứng dụng hợp lệ. Nó tốt (cả 2 mặt) nhưng nó không phải đại diện hoặc tạm thời để viết các câu truy vấn đúng.
    
    Từ [Tại sao tôi ghét DISCTINCT](http://weblogs.sqlteam.com/markc/archive/2008/11/11/60752.aspx):
    
    > Where things start to go sour in my opinion is when a developer is building substantial query, joining tables together, and all of a sudden he realizes that it **looks** like he is getting duplicate (or even more) rows and his immediate response...his "solution" to this "problem" is to throw on the DISTINCT keyword and **POOF** all his troubles go away.
    > Mọi thứ bắt đầu dến trong tâm trí tôi khi 1 nhà phát triển bắt đầu viết câu truy vấn đáng kể, nối các bảng lại với nhau, và bỗng nhiên anh ý nhận ra nó **trông** như kiểu ông ý đang lấy các dòng trùng nhau(thậm chí nhiều hơn) và câu trả lời ngay lập tức của anh ý ..... "cách giải quyết" của anh ý cho "vấn đề" này là ném từ khóa DISTINCE và **ném** tất cả các rắc rối đi.
    
    **5. Thích các phép gộp hơn và phép nối**
    Một trong những lỗi phổ biến của các nhà phát triển ứng dụng cơ sở dữ liệu là không nhận ra phép hợp(ví dụ như mệnh đề GROUP BY) tốn kém hơn thế nào so với các phép nối.
    
    Để cho bạn 1 ý tưởng về điều này phổ biến rộng rãi như thế nào, tôi đã viết về chủ đề này vài lần ở đây và đã bị vote down rất nhiều. Ví dụ:
    
    Từ [câu lệnh SQL  - “join” với “group by và having”](https://stackoverflow.com/questions/477006/sql-statement-join-vs-group-by-and-having/477013#477013):
    
    > câu truy vấn đầu tiên:
    
    SELECT userid
    FROM userrole
    WHERE roleid IN (1, 2, 3)
    GROUP by userid
    HAVING COUNT(1) = 3
    
    > thời gian truy vấn: 0.312 s
    
    > câu truy vấn thứ 2:
    
    SELECT t1.userid
    FROM userrole t1
    JOIN userrole t2 ON t1.userid = t2.userid AND t2.roleid = 2
    JOIN userrole t3 ON t2.userid = t3.userid AND t3.roleid = 3
    AND t1.roleid = 1
    
    > thơi gian truy vấn: 0.016 s
    
    > Đúng vậy. Phiên bản nối tôi đề xuất **nhanh gấp 2 lần phiên bản nhóm.**
    
    **6. Không đơn giản hóa các câu truy vấn phức tạp qua view**
    
    Không phải tất cả các nhà cung cấp cơ sở dữ liệu hỗ trợ viewiew nhưng chúng đều cóó thể đơn giản hóa các câu truy vấn nếu sử dụng 1 cách khôn ngoan. Ví dụ, trong 1 dự án tôi sử dụng [generic Party model](http://www.tdan.com/view-articles/5014/) cho CRM. Đấy là 1 kỹ thuật mô hình rất mạnh và linh hoạt có thể sử dụng nhiều phép nối. Trong mô hình này có :
    
    - **Party**: con người và các tổ chức;
    - **Party Role**: các việc mà các nhóm làm , như nhân viên và nhà tuyển dụng;
    - **Party Role Relationship**: các luật liên quan thế nào với các luật khác.
    
    Ví dụ:
    
    - Ted là 1 Person, là 1 tập con Party;
    - Ted có nhiều luật, 1 trong số chúng là Employee;
    - Intel là 1 tổ chức, là 1 tập con cửa Party;
    - Intel có nhiều luật, 1 trong số chúng là Employer;
    - Intel tuyển Ted, nghĩa là có quan hệ giữa các luật tương ứng của chúng.
    
    Do vậy, có 5 bảng nối để kếtết nối Ted với các nhà tuyển dụng của anh ý.Giả định rằng các nhân viên là Persons(không phải các tổ chức) và cung cấp các view hỗ trợ
    CREATE VIEW vw_employee AS
    SELECT p.title, p.given_names, p.surname, p.date_of_birth, p2.party_name employer_name
    FROM person p
    JOIN party py ON py.id = p.id
    JOIN party_role child ON p.id = child.party_id
    JOIN party_role_relationship prr ON child.id = prr.child_id AND prr.type = 'EMPLOYMENT'
    JOIN party_role parent ON parent.id = prr.parent_id = parent.id
    JOIN party p2 ON parent.party_id = p2.id
    
    Và bỗng nhiên bạn có 1 view rất đơn giản các dữ liệu bạn muốn những ở mô hình dữ liệu linh hoạt cao.
    
    **7. Không lọc đầu vào**
    
    Đây là 1 chuyện lớn đấy. Bây giờ tôi thích PHP nhưng nếu bạn không muốn biết bạn đang làm gì nó thực sự dễ dàng trong việc tạo các site có thể bị tấn công. Không cái nào tổng kết điều này tốt hơn là [câu chuyện về BOobby Tables bé nhỏ](http://xkcd.com/327/).
    
    Dữ liệu được cung cấp bởi người dùng qua URLs, fỏm data **và cookies** nên luôn luôn được coi là không tốt và cần được lọc. Hãy đảm bảo rằng bạn đang lấy được những gì bạn mong muốn
    
    **8. Không sử dụng các lệnh được chuẩn bị**
    
    Các câu lệnh chuẩn bị là khi bạn biên dịch 1 truy vấn khuyết dữ liệu sử dụng trong các lệnh thêm, cập nhật và 
    
    mệnh đề WHERE  và sẽ cung cấp dữ liệu cho nó sau. Ví dụ:
    
    SELECT * FROM users WHERE username = 'bob'
    
    vs
    
    SELECT * FROM users WHERE username = ?
    
    or
    
    SELECT * FROM users WHERE username = :username
    
    dựa trên nền tảng của bạn.
    
    Tôi đã từng thấy các cơ sở dữ liệu bị phá hủy vì điều này. Thông thường, mỗi lần bất kỳ 1 cớ sở dữ liệu hiện đại nào gặp 1 câu truy vấn mới, nó phải biên dịch nó. Nếu nó gặp 1 câu truy vấn đã nhìn thấy trước đó, thì cơ sở dữ liệu có cơ hội cache câu trúy vấn đã biên dịch và thực hiện nó. Bằng cách thực hiện câu truy vấn nhiều lần bạn đang chó phép cơ sở dữ liệu để tìm kiếm và tối ưu thích hợp ( ví dụ, bằng cách ghim các câu truy vấn đã biên dịch trong bộ nhớ)
    
    Sử dụng các câu lệnh được chuẩn bị cũng sẽ cho bạn thống kê ý nghĩa về việc các câu truy vấn cụ thể được sử dụng với tần suất nào.
    
    Các câu lệnh được chuẩn bị cũng bảo vệ bạn tốt hơn với cách tấn công SQL injection
    
    **9. Không chuẩn hóa đủ**
    
    [Chuẩn hóa cơ sở dữ liệu](http://en.wikipedia.org/wiki/Database_normalization) về cơ bản là quá trình tối ưu thiết kế cơ sở dữ liệu hoặc là cách bạn tổ chức dữ liệu của bạn thành các bảng.
    
    Tuần này tôi bắt gặp code của ai đó mà họ tách 1 mảng ra và thêm chúng vào 1 trường đơn lẻ trong 1 cơ sở dữ liệu. Quả trình chuẩn hóa nên là coi các phần tử của mảng như là 1 dòng riêng biệt trong bảng con (ví dụ quan hệ một-nhiều)
    
    Nó cũng từng được đề cập trong [Phương thức tốt nhất để lưu 1 danh sách id người dùng](https://stackoverflow.com/questions/620645/best-method-for-storing-a-list-of-user-ids):
    
    > Tôi đã từng thấy 1 hệ thống khác lưu trữ danh sách trong mảng tuần tự PHP
    
    Nhưng phần còn lại của quá trình chuẩn hóa đến từ rất nhiều mẫu.
    
    Đọc thêm:
    
    - [Chuẩn hóa: Bao nhiêu thì đủ?](http://www.techrepublic.com/article/normalization-how-far-is-far-enough/)
    - [SQL qua Design: Tại sao cần chuẩn hóa cơ sở dữ liệu ](http://www.sqlmag.com/Article/ArticleID/4887/sql_server_4887.html)
    
    **10. Chuẩn hóa quá nhiều**
    
    Điều này nghe có vè mâu thuẫn với điều trươc, nhưng cũng giống như nhiều thứ khác, chuẩn hóa cũng chỉ là 1 công cụ. Nó sẽ chẳng có 1 điểm kết cụ thể nào. Tôi nghĩa rằng nhiều nhà phát triển quên mất điều này và bắt đầu coi "công cụ" như là 1 "kết thúc". Kiểm thử đơn vị là 1 ví dụ cơ bản cho điều này.
    
    Khi tôi làm việc trên 1 hệ thống có hệ thống phân cấp rõ ràng cho khách hàng tôi đã gặp 1 vài điều kiểu như thế này:
    
    Licensee -&gt;  Dealer Group -&gt; Company -&gt; Practice -&gt; ...
    
    như thể là bạn phải nối khoảng 11 bảng với nhau trước khi bạn muốn lấy bất kỳ dữ liệu có nghĩa nào. Đây là 1 ví dụ tốt về việc chuẩn hóa quá nhiều
    
    Thêm vào đó, việc tiêu chuẩn hóa lại 1 cách cẩn thận và được lưu tâm có thể mang lại những lợi ích đáng kể nhưng bạn phải thực sự cẩn thận khi làm điều này.
    
    Đọc thêm:
    
    - [Vì sao chuẩn hóa quá nhiều lại là điều không tốt](http://www.selikoff.net/blog/2008/11/19/why-too-much-database-normalization-can-be-a-bad-thing/)
    - [Chuẩn hóa cơ sở dữ liệu nhiều như thế nào?](https://stackoverflow.com/questions/496508/how-far-to-take-normalization-in-database-design)
    - [Khi nào thì không chuẩn hóa cơ sở dữ liệu SQL của bạn](http://www.25hoursaday.com/weblog/CommentView.aspx?guid=cc0e740c-a828-4b9d-b244-4ee96e2fad4b)
    - [Có thể chuẩn hóa lại không bình thường](http://www.codinghorror.com/blog/archives/001152.html)
    - [Nguyên nhân các cuộc tranh luận chuẩn hóa cơ sở dữ liệu trên Coding Horror](http://highscalability.com/mother-all-database-normalization-debates-coding-horror)
    
    **11. Sử dụng exclusive arcs**
    
    1 exclusive arc là 1 lỗi phổ biến khi 1 bảng được tạo với 2 hoặc nhiều hơn khóa ngoai khi 1 và 1 chỉ 1 trong số chúng có thể không null. **Sai lầm lớn.** Đây là lý do nó trở bên khó khăn hơn trong việc bảo trì toàn vẹn dữ liệu. Sau cùng, ngay cả khi với các ràng buộc tham chiếu, không gì ngăn việc 2 hoặc nhiều hơn các khóa ngoại này được đặt (mặc dù kiểm tra ràng buộc phức tạp)
    
    Từ [Hướng dẫn thực hành với thiết kế cơ sở dữ liệu quan hệ](http://books.google.com.au/books?id=7ZAk0YiKQV0C&pg=PA110&lpg=PA110&dq=%22exclusive+arc%22+database&source=bl&ots=AyNPWsac__&sig=gBFIerXckQlVpRdd6ycI5JEgq3U&hl=en&ei=PzGzSZfrFcPVkAWWyZDZBA&sa=X&oi=book_result&resnum=1&ct=result):
    
    > Chúng tôi không khuyến khích xây dựng exclusive bất cứ khi nào có thể, vì 1 lý do rằng nó có thể lùng tùng khi viết code và làm việc bảo trì khó khă.n
    
    **12. Không phân tích hiệu suất trên các câu lệnh**
    
    Theo chủ nghĩa thực dụng tối cao, đặc biệt là trong cơ sở dữ liệu. Nếu bạn vẫn đang bám dính lấy nguyên tắc rằng chúng đã trở nên độc đoán thì bạn thực sự đang mắc sai lầm đấy. Lấy 1 ví dụ về các truy vấ gộpn bên trên. Phiên bản gộp có vẻ nhìn "ổn" nhưng hiệu suất của nó thì tệ. Việc so sanh về hiệu suất nên kết thúc cuộc tranh luận (nhưng nó không) nhưng thêm 1 điều rằng : việc bắn quá nhiều view thống báo xấu ngay trong vị trí đầu tiên là ngu dốt, thậm chí là nguy hiểm
    
    **13. Quán tin vào UNION ALL và đặc biệt là cấu trúc UNION**
    
    1 UNION trong điều kiện SQL chỉ đơn thuần là nối các tập dữ liệu phù hợp, nghĩa là có cùng loại và cùng số cột. Sự khác nhau giữa chúng là UNION ALL là phép nối đơn thuần và nên được ưu tiên khi có thể, khi mà UNION sẽ thực hiện ngầm DISCTINCT để loại các dòng trùng lặp.
    UNIONs, như DISTINCT, có vai trò của riêng chúng. Đều có các ứng dụng hợp lệ. Nhưng nếu bạn tự tìm hiểu chúng nhiều 1 chút, đặc biệt là trong các truy vấn con, thì chúng thực sự đang làm sai.  Đó có thể là  trường hợp của cấu trúc trúy vấn tệ hoặc là 1 mô hình dữ liệu thiết kế kém khiến bạn phải làm những điều này.
    
    UNIONs, đặc biệt là khi sử dụng trong phép nối hoặc các truy vấn con phụ thuộc, có thể làm hỏng cơ sở dữ liệu. Cố gắng tranh sử dụng chúng nếu có thể.
    
    **14. Sử dụng điều kiện OR trong các câu truy vấn**
    
    This might seem harmless. After all, ANDs are OK. OR should be OK too right? Wrong. Basically an AND condition **restricts** the data set whereas an OR condition **grows** it but not in a way that lends itself to optimisation. Particularly when the different OR conditions might intersect thus forcing the optimizer to effectively to a DISTINCT operation on the result.
    Điều này trông có vẻ như vô hại. Sau cùng, thì ANDs thì ốt. OR cũng nên tốt phải không? Không đúng. Thông thường 1 điều kiện AND **hạn chế** tập dữ liệu trong khi điều kiện OR **làm tăng* nó nhưng không phải theo cách  khiến chúng được tối ưu. Đặc biệt khi các điều kiện  OR khác nhau có thể giao nhau khiến chó việc tối ưu hóa hiệu quả với các phép DISTINCT trong kết quả
    Tệ:
    
    ... WHERE a = 2 OR a = 5 OR a = 11
    
    Tốt:
    
    ... WHERE a IN (2, 5, 11)
    
    Bây giờ việc tối ưu SQL của bạn có thể hiệu quả từ câu trúy vấn đầu tiên  tới cái thứ 2. Nhưng có thể không. Đừng làm như thế.
    
    **15. Không thiết kế mô hình dữ liệu để có giải pháp hiệu suất cao**
    
    Đây là điểm rất khó để định lượng. Nó thường được theo dõi hiệu quả của nó. Nếu bạn thấy mình đang biết các câu truy vấn cho các việc tương đối đơn giản hay các truy vấn để tìm các thông tin tương đối đơn giản nhưng không hiệu quả, thì bạn chắc chắn đang có mô hình dữ liệu tệ.
    
    Bằng 1 cách nào đó, điều này tổng kết tất cả các điều trước đó nhưng nó có thêm 1 cảnh bảo là những việc như tối ưu truy vấn thường xong đầu tiên trong khi điều này nên được hoàn thành thứ 2. Đầu tiên và cũng là quan trọng nhất là bạn nên chắc rằng bạn có 1 mô hình dữ liệu tốt trước khi cố gắng tối ưu hiệu suất. Và Knuth nói rằng:
    > Tối ưu sớm là gốc rễ của mọi điều xấu.
    
    **16. Sử dụng Database Transactions sai **
    
    Tất cả thay đổi dữ liệu cho 1 quá trình cụ thể nên atomic. Vị dụ. Nếu hoạt động thành công, nó sẽ thực hiện đầy đủ. Còn nếu nó thật bại, thì dữ liệu vấn không thay đổi. Không nên để xảy ra khả năng thay đổi "1 nửa"
    
    Lý tưởng thì cách tốt nhất để đạt được điều này là thiết kế toàn bộ hệ thống nên cố gắng hỗ trợ toàn bộ thay đổi dữ liệu qua từng câu lệnh INSERT/UPDATE/DELETE đơn. Trong trường hợp này, không có xử lý transaction đặc biệt nào cần thiết cả, vì engine cơ sở dữ liệu của bạn nên làm điều này tự động.
  
    Tuy nhiên, nếu bất kỳ tiến trình nào yêu cầ nhiều câu lệnh được thực hiện như là đơn vị để giữ dữ liệu ở trạng thái thích hợp, khi đó, Transaction Control thích hợp là cần thiết.
    
    - Bắt đầu Transaction trước câu lệnh đầu tiên.
    - Commit Transaction sau câu lệnh cuối cùng.
    - Khi có bất kỳ lỗi nào, tiến hành Rollback Transaction. Và bất kỳ NB nào! Đừng quên bỏ qua/ dừng tất cả các câu lệnh sau các lỗi.
    
    Chủ ý để ý cẩn thận tới (Subtleties: bản gốc tiếng anh sai xừ rồi) sự khôn khéo trong việc cơ sở dữ liệu của bnaj kết nối các lớp, và engine cơ sở dữ liệu tương tác với vấn đề này.
    **17. Không hiểu mô hình 'set-based'**
    
    Ngôn ngữ SQL tuân theo mô hình cụ thể phù hợp với các loại vấn đề cụ thể. Mặc dù có nhiều phần mở rộng cung cấp cụ thể, cuộc đấu tranh ngôn ngữ đẻ giải quyết vấn đề này là không đáng kể trong ngôn ngữ như Java, C#, Delphi vân vân
    
    Phần còn lại được biểu hiện trong 1 vài cách.
    
    - Áp đặt quá nhiều các phương pháp và logic bắt buộc không phù hợp trên cơ sở dữ liệu
    - Sử dụng con trỏ không phù hợp hoặc 1 cách quá đáng. Đặc biệt khi câu truy vấn đơn đầy đủ.
    - Giả định sai rằng trigger thực hiện ngay khi mỗi dòng bị ảnh hưởng trong cập nhiều đa-dòng
    
    Mục đích lọc sự phân chia trách nhiệm, và cố gắng sử dụng các công cụ thích hợp để giải quyết từng vấn đề.
    
    
    
