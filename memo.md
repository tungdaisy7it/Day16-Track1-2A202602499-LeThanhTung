# Memo Teardown — CURSOR (Anysphere)

**Họ tên:** Lê Thanh Tùng — MSV 2A202602499

**Vì sao chọn sản phẩm này:** Cursor là case hiếm có, đi đủ cả vòng tranh luận "wrapper hay moat". Ban đầu nó là một bản fork VS Code gọi model của OpenAI/Anthropic. Sau đó nó tự train model, bị chính nhà cung cấp model cạnh tranh trực diện (Claude Code, Codex), rồi bị SpaceX/xAI mua lại với giá $60B (08/2026). Timeline dày, có changelog và blog chính thức cho từng mốc, nên dựng được chuỗi quyết định thay vì phải đoán.

> Mốc thời gian của memo: 02/10/2026. Mọi số liệu lấy theo nguồn ghi trong từng hàng.

---

## §1. Timeline các cập nhật lớn

| # | Thời điểm | Cập nhật (quyết định sản phẩm) | Context lúc đó | Nguyên lý |
|---|---|---|---|---|
| 1 | 01–03/2023 | **Ra mắt Cursor: một editor riêng (fork VS Code), không làm extension.** Trước đó founders từng thử AI cho CAD rồi bỏ. — [Latent Space podcast với Aman Sanger, 08/2023](https://www.latent.space/p/cursor) | ChatGPT vừa ra (11/2022), GPT-4 ra 03/2023. GitHub Copilot đang thống trị dưới dạng extension, và chính GitHub phải sửa source VS Code để có ghost text nhiều dòng. Anysphere lúc này là 4 sinh viên MIT. | **Platform risk → sở hữu bề mặt tương tác (x10 không phải x2).** "Không thể có thế giới mà LLM viết 90–95% code mà vẫn giữ form factor cũ." Extension chỉ cho cải tiến x2. Muốn đổi UX ở mức x10 thì phải kiểm soát cả IDE. Fork VS Code giữ switching cost vào ≈ 0 (extension, keybinding giữ nguyên). |
| 2 | 08/2024 | **Series A $60M, công bố stack model tự build: next-edit prediction (Cursor Tab), retrieval hàng tỷ file, fast-apply bằng speculative decoding.** — [Cursor blog "Series A and Magic"](https://cursor.com/blog/series-a) | Claude 3.5 Sonnet (06/2024) đưa chất lượng code của model nền lên một bậc, và Cursor bùng nổ trên AI Twitter. Lúc này có hơn 40.000 khách hàng. | **Wrapper → moat bằng "model chuyên dụng ở chỗ nhạy độ trễ" + vòng lặp học.** Việc suy luận nặng thì thuê model frontier. Việc cần dưới 100ms (Tab, apply) thì tự train, vì mỗi lần user accept/reject một gợi ý Tab là thêm một điểm dữ liệu mà OpenAI/Anthropic không có. |
| 3 | 11/2024 | **Mua Supermaven (12/11) và ra Composer Agent ở bản 0.43 (24/11): agent tự chọn context và chạy terminal.** — [Cursor blog Supermaven](https://cursor.com/blog/supermaven) · [Changelog 0.43](https://cursor.com/changelog/0-43-x) | Claude 3.5 Sonnet bản mới (10/2024) mạnh vượt trội về agentic coding. Windsurf (Codeium) ra editor có agent "Cascade" ngày 13/11/2024, tức là 11 ngày trước Cursor. Copilot chưa có agent. | **Định nghĩa lại "tốt": từ "gợi ý đúng dòng" sang "giao việc, nhận diff".** Đơn vị giá trị đổi từ *keystroke* sang *task*. Supermaven là phép cộng dồn cho moat Tab: đội của Supermaven nói "extension APIs đang chặn" họ, nghĩa là nguyên lý ở mốc 1 được xác nhận lại. |
| 4 | 06/2025 | **Cursor 1.0 (04/06): BugBot review PR trên GitHub, Background Agent cho mọi user, Memories, cài MCP một chạm.** Cùng tuần đó gọi Series C $900M, hơn $500M ARR, hơn nửa Fortune 500 dùng. — [Changelog 1.0](https://cursor.com/changelog/1-0) · [Tweet Series C của Cursor](https://x.com/cursor_ai/status/1931032123038945530) | Claude Code GA (05/2025) và Copilot agent mode đưa agent ra *ngoài* editor (terminal, GitHub). Khách enterprise bắt đầu mua seat theo team. | **Mở rộng dọc vòng đời phần mềm (SDLC): từ "viết" sang "review" và "chạy nền".** Khi code do agent viết, nút thắt chuyển sang khâu review. Ai sở hữu khâu review sẽ ngồi trong workflow của cả team chứ không chỉ của từng dev, và switching cost chuyển từ cá nhân sang tổ chức. |
| 5 | 06/2025 | **Đổi pricing (16/06): bỏ "500 request/tháng", Pro thành "$20 usage theo giá API + unlimited Tab/Auto", ra gói Ultra $200. Ngày 04/07 xin lỗi và hoàn tiền.** — [Cursor blog "Clarifying our pricing"](https://cursor.com/blog/june-2025-pricing) | Agent chạy dài nên một request khó "tốn gấp cả chục lần request dễ" (trích blog). Anthropic vừa là nhà cung cấp chính vừa ra Claude Code với gói Max $100/$200. | **Unit economics của wrapper: giá phải bám chi phí inference (pass-through), và chỉ model của mình mới "unlimited" được.** Cố ý chỉ để Auto/Tab unlimited là đòn bẩy để kéo traffic sang model mình kiểm soát chi phí, đặt nền cho mốc 6. |
| 6 | 10/2025 | **Cursor 2.0 (29/10): model tự train đầu tiên là Composer ("4x nhanh hơn model cùng tầm"), và giao diện multi-agent chạy song song qua git worktree.** — [Cursor blog 2.0](https://cursor.com/blog/2-0) | Claude Sonnet 4.5 (09/2025) và Codex cạnh tranh trực diện. Margin bị kẹp vì nhà cung cấp model cũng là đối thủ. Đồng sáng lập Arvid Lunnemark rời công ty. | **Tích hợp dọc xuống tầng model: vòng lặp học khép kín.** Dữ liệu tương tác agent trong Cursor dùng để RL ra model riêng, model riêng thì rẻ và nhanh hơn, nên giữ được margin. Đây là bước rời hẳn vị thế "wrapper". (Composer 2, 03/2026, sau đó bị phát hiện là xây trên Kimi K2.5 mà không công bố: [VentureBeat](https://venturebeat.com/technology/cursors-composer-2-was-secretly-built-on-a-chinese-ai-model-and-it-exposes-a). Moat nằm ở dữ liệu RL chứ không ở base model.) |
| 7 | 04/2026 | **Cursor 3 (02/04): "New Cursor Interface" lấy agent làm trung tâm (Agents Window, chạy agent song song ở local/worktree/cloud/SSH, `/best-of-n` so nhiều model).** — [Changelog 3.0](https://cursor.com/changelog/3-0) | Agent chạy nhiều giờ đã thành chuẩn ngành. Cuộc đua chuyển từ "model nào giỏi nhất" sang "quản lý nhiều agent thế nào". | **Đổi vai người dùng từ người viết code sang người điều phối và review (manager of agents).** Editor lùi xuống thành công cụ phụ. `/best-of-n` biến việc *trung lập về model* thành tính năng: Cursor là nơi so sánh model, điều mà lab nào cũng không muốn làm. |
| 8 | 04–08/2026 | **Bán cho SpaceX/xAI: quyền chọn mua tháng 04, thực thi ngày 16/06, đóng ngày 14/08/2026 ($60B, all-stock).** Sản phẩm đầu tiên là Grok 4.6 đồng phát triển với Cursor. Ngày 29/08 OpenAI tuyên bố cắt quyền truy cập model từ 12/11/2026. Anthropic cam kết tiếp tục cung cấp. — [Wikipedia (tổng hợp, có trích nguồn)](https://en.wikipedia.org/wiki/Cursor_(company)) · [Techzine: hoàn tất thương vụ](https://www.techzine.eu/news/devops/143619/spacex-completes-acquisition-of-cursor/) · [CNBC 29/08/2026](https://www.cnbc.com/2026/08/29/openai-cursor-spacex-model-access.html) | Cursor khoảng $3B ARR (05/2026), nhưng nguồn compute và model phụ thuộc vào chính đối thủ. xAI cần distribution trong giới kỹ sư, Cursor cần GPU (Colossus). | **Compute và model là đầu vào chiến lược → tích hợp dọc với một lab.** Đây là kết cục logic của chuỗi 2 → 5 → 6. Cái giá là mất tính trung lập: OpenAI rút model, và chính "điểm mạnh model-agnostic" ở mốc 7 bị lung lay. |

**Vì sao chọn những mốc này:** Mình giữ những mốc làm đổi *đơn vị giá trị* hoặc *vị trí trong chuỗi giá trị*: chọn editor (1), tự train model nhỏ (2), từ dòng code sang task (3), từ cá nhân sang team/SDLC (4), từ flat-fee sang theo compute (5), từ thuê model sang có model (6), từ file sang agent (7), từ startup độc lập sang thuộc một lab (8). Ba nhóm mốc đã cân nhắc rồi loại:

- **Seed $8M từ OpenAI Startup Fund (10/2023):** là sự kiện tài chính, không đổi gì trong sản phẩm.
- **Cursor CLI (08/2025)** và **mua Graphite (12/2025):** đều là hệ quả của nguyên lý đã có ở mốc 4. CLI là nước *theo sau* Claude Code, Graphite là đào sâu khâu review mà BugBot đã mở. Đưa vào chỉ lặp lại nguyên lý.
- **Sự cố bot hỗ trợ "Sam" bịa chính sách (04/2025)** và **các bản 09/2026 (Projects, Rollouts, Security Review):** cái đầu là sự cố vận hành. Cái sau còn quá mới để biết có thành bước ngoặt không, nên mình dùng nó làm *tín hiệu cho dự đoán* ở §3.

---

## §2. Tệp user & JTBD

| | Early adopters (2023 – giữa 2024) | Tệp hiện tại (2025 – 2026) |
|---|---|---|
| **Đặc điểm** | Full-stack/frontend dev ở startup dưới 20 người hoặc indie hacker, đã dùng VS Code + Copilot, trả tiền ChatGPT Plus, theo dõi AI Twitter (kiểu người như Alex MacCaw, người khen "best product I've used in a while" trong [podcast Latent Space](https://www.latent.space/p/cursor)). Tự quyết định công cụ, tự trả $20 bằng thẻ cá nhân. | (a) **Kỹ sư trong đội sản phẩm ở công ty lớn** (hơn nửa Fortune 500, như NVIDIA, Uber, Adobe theo [tweet Series C](https://x.com/cursor_ai/status/1931032123038945530)). Công ty mua seat Teams $40/user, engineering manager/platform team quyết định. (b) **Power user chạy nhiều agent song song**, trả Ultra $200, dùng song song cả Claude Code. |
| **JTBD chính** | "Viết xong một tính năng mới ở codebase nhỏ mà không phải copy-paste qua lại giữa ChatGPT và editor." "Hiểu nhanh một thư viện lạ để ship trong ngày." | (a) "Hoàn thành ticket trong codebase hàng triệu dòng mà không phá thứ khác, và qua được review của đồng nghiệp." (b) "Giao nhiều việc cùng lúc cho agent rồi chỉ phải review diff, để một người làm được sản lượng của cả nhóm." Phía manager: "Tăng throughput của team mà không giảm chất lượng code đã merge." |
| **Trước đó họ làm bằng cách nào** | Copilot autocomplete từng dòng, cộng với tab ChatGPT để hỏi và dán code qua lại, cộng Stack Overflow. | (a) Copilot do công ty mua sẵn, review PR thủ công, đọc code bằng grep/IDE truyền thống. (b) Tự viết, hoặc chia việc cho dev junior. |

**Dịch chuyển tệp:** Có hai mốc gây ra dịch chuyển.

- **Mốc 3 (Agent, 11/2024)** đổi JTBD từ "gõ nhanh hơn" sang "giao việc". Khi việc được giao là cả task, giá trị tăng theo độ lớn của codebase, nên tệp có codebase lớn (doanh nghiệp) mới là tệp nhận nhiều giá trị nhất.
- **Mốc 4 (Cursor 1.0, 06/2025)** đưa sản phẩm ra khỏi máy cá nhân, vào GitHub PR (BugBot) và cloud (Background Agent). Người mua lúc này là *tổ chức*: tính năng chạm vào workflow chung của team, nên quyết định mua chuyển từ dev lên manager. Con số "hơn nửa Fortune 500" công bố cùng tuần là bằng chứng của dịch chuyển này.
- **Mốc 5 (pricing 06/2025)** tách tệp (b): ai chạy agent nặng thì bị đẩy lên Ultra, hoặc chuyển sang Claude Code. Thread "switch sang Claude Code vì hết quota" xuất hiện nhiều từ đây ([ví dụ tổng hợp](https://theaiengineer.substack.com/p/cursor-vs-claude-code)).

**Switching cost (map 4 forces):**

| Lực | Hướng | Nội dung cụ thể với Cursor |
|---|---|---|
| **Push** (đẩy khỏi cách cũ) | → vào Cursor | Copilot thời đầu chỉ autocomplete. Copy-paste với ChatGPT mất ngữ cảnh codebase. |
| **Pull** (hút về Cursor) | → vào Cursor | Tab đoán *lần sửa tiếp theo* (không chỉ dòng tiếp theo), index cả codebase, chọn được nhiều model, chạy agent song song. |
| **Anxiety** (lo ngại khi dùng) | ← kéo ra | Giá khó đoán (vụ 06/2025). Lo về quyền riêng tư code. Nguồn gốc model không minh bạch (Composer 2/Kimi). Từ 08/2026 thêm lo ngại dữ liệu code đi qua một công ty của Musk, và mất model OpenAI từ 12/11/2026. |
| **Habit/Inertia** (thói quen giữ lại) | giữ ở lại | Phản xạ bấm Tab đã thành thói quen. Rules/memories riêng của dự án. Ở tầng doanh nghiệp: hợp đồng seat, SSO/SCIM, BugBot đã cắm vào quy trình PR. |

**Lực nào giữ user mạnh nhất?** Ở *cá nhân*, mình cho là **Pull của trải nghiệm (Tab + agent UX)**, chứ không phải lock-in dữ liệu. Code nằm trong git, còn settings và extension VS Code mang đi được, nên switching cost *ra* cũng gần bằng 0, y như switching cost *vào* (đó là cái giá của chính quyết định fork VS Code ở mốc 1). Nếu lực Pull này mất đi, tức là đối thủ cùng dùng một model nền và UX ngang bằng, user cá nhân sẽ đi rất nhanh. Bằng chứng: tệp (b) bỏ sang Claude Code ngay khi pricing bất lợi. Điều này giải thích vì sao từ mốc 4 trở đi Cursor dồn lực vào *switching cost cấp tổ chức* (BugBot, Graphite, Rollouts) và *model riêng* (Composer). Hai thứ đó là thứ đối thủ không copy được bằng cách dùng chung model.

---

## §3. Ba dự đoán hướng đi (10/2026 → 10/2027)

**Dự đoán 1** *(loại: thay đổi mô hình kiếm tiền + phản ứng với đe dọa Big Tech)*
- **Dự đoán:** Trong 6 tháng, Cursor đặt model của SpaceXAI (Grok/Composer) làm mặc định cho Auto và cho gói giá rẻ/unlimited. Model bên thứ ba (Claude) chuyển thành "premium usage" tính theo giá API. Model OpenAI biến mất khỏi danh sách sau 12/11/2026 mà không có bản thay thế tương đương.
- **Lập luận:** Mốc 5 cho thấy Cursor đã thử dùng giá để lái traffic về model mình kiểm soát chi phí ("unlimited chỉ cho Auto"). Mốc 6 đã có model riêng, và mốc 8 nay có thêm GPU Colossus nên chi phí biên của model nhà gần như thấp nhất thị trường. OpenAI đã ấn định ngày cắt. Model nhà chỉ chiếm phần lớn traffic thì margin mới thoát được vòng kẹp "nhà cung cấp kiêm đối thủ" ở §1.

**Dự đoán 2** *(loại: mở rộng tính năng, ăn trọn SDLC cho enterprise)*
- **Dự đoán:** Trong 12 tháng, Cursor gộp BugBot, Graphite, Rollouts và Security Review thành một gói enterprise. Gói này nhận ticket từ Jira/Linear, để agent làm từ code đến review, deploy và theo dõi sau deploy, và tính phí theo *agent/usage* bên cạnh phí theo seat.
- **Lập luận:** Chuỗi mốc 4 → Graphite (12/2025) → Projects/Rollouts/Security Review (09/2026, [changelog](https://cursor.com/changelog)) liên tục kéo dài phần SDLC mà Cursor sở hữu. §2 cho thấy lock-in ở cá nhân yếu, còn ở tổ chức thì mạnh. Tệp hiện tại là doanh nghiệp, người ra quyết định là manager, và JTBD của manager là "tăng throughput mà không giảm chất lượng merge". Một gói end-to-end trả lời đúng JTBD đó. Còn phí theo seat thì không còn hợp lý khi một người điều khiển mười agent (mốc 7).

**Dự đoán 3** *(loại: mở rộng segment, sang người "không phải kỹ sư")*
- **Dự đoán:** Cursor ra một sản phẩm/plan trên web dành cho PM, designer và founder không code (luồng "Start from Scratch": không cần GitHub, preview trực tiếp, publish lên Vercel), cạnh tranh trực diện với Lovable/v0/Replit. Sản phẩm này sẽ được phân phối qua hệ sinh thái X/Grok.
- **Lập luận:** Changelog 27/08/2026 đã bỏ điều kiện phải có GitHub và tự tạo repo "Cursor Origin" kèm live preview, tức là đã gỡ rào cản lớn nhất với người không phải dev. Mốc 7 đẩy editor xuống vai phụ, nên giao diện agent không còn đòi hỏi biết đọc code. Sau mốc 8, chủ mới (xAI) cần tăng số người dùng đại trà hơn là tăng seat kỹ sư. Tệp early adopter dev cá nhân thì đã bão hòa và dễ bỏ đi (§2), nên tăng trưởng phải tìm tệp mới.

**Tự tin nhất:** Dự đoán 2. Nó là phép ngoại suy của một chuỗi mốc đã nhất quán suốt 15 tháng, và khớp với tệp đang trả tiền. **Giả định nếu sai sẽ làm nó gãy:** doanh nghiệp vẫn tin giao code và pipeline deploy cho một công ty con của SpaceX/xAI. Nếu khối Fortune 500 (đặc biệt nhóm tài chính và quốc phòng ngoài hệ Musk) bắt đầu cấm Cursor vì lo ngại quản trị dữ liệu, giống lý do OpenAI nêu khi rút model, thì Cursor sẽ phải quay về tệp cá nhân/SMB. Khi đó dự đoán 3 trở thành hướng chính.

---

## §4. AI Log

> Khai trung thực: bản nháp memo này do **Claude Code (Claude Opus 5.5)** dựng trong một phiên làm việc. AI dùng WebSearch/WebFetch để mở trực tiếp các trang nguồn. Cột 3 ghi lại việc đã kiểm chứng trong phiên và những việc **tôi phải tự làm lại trước khi nộp**.

| Việc | AI làm hay bạn làm? | Bạn kiểm chứng/phán đoán lại thế nào? |
|---|---|---|
| Chọn sản phẩm (Cursor) theo 3 tiêu chí | AI đề xuất, tôi chốt | Kiểm tra cả 3 tiêu chí: AI là lõi sản phẩm; changelog, blog và trang Product Hunt đều tồn tại; JTBD rõ. |
| Gom nguồn thô và mốc ứng viên (~20 mốc, xem Phụ lục) | AI (web search + fetch changelog, blog Cursor, Wikipedia, báo) | AI đã mở trực tiếp: changelog 0.43, 1.0, 2.0, 3.0; blog Series A, Supermaven, pricing. Riêng **CNBC trả lỗi 403**, nên tin OpenAI rút model mới chỉ đối chiếu qua Techzine và Wikipedia. *Tôi cần tự mở lại từng link trong §1 trước khi nộp.* |
| Kiểm chứng sự kiện 2026 nằm ngoài hiểu biết sẵn có của AI (Cursor 3, Composer 2/Kimi, thương vụ SpaceX, OpenAI rút model) | AI tìm, đối chiếu | Mỗi sự kiện được đối chiếu với ít nhất 2 nguồn độc lập (ví dụ thương vụ SpaceX: Wikipedia + Techzine + DevOps.com). Không đưa vào memo các con số chỉ có một nguồn blog SEO (như "$4B ARR", "projected $6B"). |
| Chọn 8 mốc và loại các mốc còn lại | AI đề xuất, tôi duyệt | Tiêu chí: mốc phải đổi đơn vị giá trị hoặc vị trí trong chuỗi giá trị. Đã loại seed round (tài chính), CLI và Graphite (lặp nguyên lý), sự cố "Sam" (vận hành), bản 09/2026 (quá mới). |
| Revert nguyên lý cho từng mốc | AI nháp, tôi chỉnh | Đối chiếu với khái niệm đã học (x10, wrapper/moat, vòng lặp học, định nghĩa "tốt", Vertical AI). *Phần này AI làm thay nhiều nhất. Tôi phải tự giải thích lại được từng ô trước khi nộp.* |
| Tệp user, JTBD và 4 forces | AI nháp từ nguồn (podcast, tweet Series C, bài so sánh Cursor/Claude Code) | Chân dung early adopter suy ra từ podcast 2023 và lời khen công khai, **không phải khảo sát**. Đây là suy luận, nên cần tôi đối chiếu thêm bằng review G2 hoặc Reddit, hoặc tự dùng thử bản free. |
| 3 dự đoán | AI nháp, tôi phản biện | Kiểm tra mỗi lập luận phải trỏ về ít nhất 1 mốc ở §1 và 1 nhận định ở §2. Đã nêu giả định gãy cho dự đoán tự tin nhất. |

---

### Phụ lục — Nguồn thô và mốc ứng viên (Step 0)

Mốc ứng viên đã xem xét:

- 2022: thành lập, thử AI cho CAD.
- 01/2023: công bố. 03/2023: ra mắt.
- 08/2023: podcast Latent Space.
- 10/2023: seed $8M ([TechCrunch](https://techcrunch.com/2023/10/11/anysphere-raises-8m-from-openai-to-build-an-ai-powered-ide)).
- Cuối 2023: Copilot++, sau đổi tên Cursor Tab.
- 08/2024: Series A.
- 11/2024: Supermaven, 0.43 Agent; định giá khoảng $2.5B.
- 01/2025: $100M ARR.
- 04/2025: sự cố bot "Sam".
- 06/2025: 1.0, Series C, pricing mới.
- 07/2025: BugBot GA, đội Koala.
- 08/2025: CLI.
- 10/2025: 2.0/Composer, Cloud Agents, Arvid rời công ty.
- 11/2025: Series D $2.3B @ $29.3B, hơn $1B ARR.
- 12/2025: Graphite.
- 03/2026: Composer 2/Kimi.
- 04/2026: Cursor 3, quyền chọn SpaceX.
- 05/2026: $3B ARR.
- 06/2026: SpaceX thực thi quyền mua.
- 08/2026: đóng thương vụ, Grok 4.6, "Start from Scratch", OpenAI rút model.
- 09/2026: Projects, Self-hosted Machines, Rollouts, Security Review.

Nguồn chính:

- [Changelog Cursor](https://cursor.com/changelog)
- [Blog Cursor](https://cursor.com/blog)
- [Product Hunt — Cursor](https://www.producthunt.com/products/cursor)
- [Latent Space podcast](https://www.latent.space/p/cursor)
- [Wikipedia — Cursor (company)](https://en.wikipedia.org/wiki/Cursor_(company))
- [Techzine](https://www.techzine.eu/news/devops/143619/spacex-completes-acquisition-of-cursor/)
- [SiliconANGLE — Graphite](https://siliconangle.com/2025/12/19/cursor-acquires-ai-code-review-startup-graphite/)
- [VentureBeat — Composer 2/Kimi](https://venturebeat.com/technology/cursors-composer-2-was-secretly-built-on-a-chinese-ai-model-and-it-exposes-a)

### Checklist tự kiểm tra

- [x] Tên sản phẩm ở đầu memo
- [x] §1: 8 mốc, mỗi mốc đủ 4 cột + link nguồn, nguyên lý có tên
- [x] §1: giải thích các mốc đã loại
- [x] §2: bảng early adopters vs hiện tại + dịch chuyển nối về mốc §1 + 4 forces
- [x] §3: 3 dự đoán, mỗi cái có dự đoán + lập luận dẫn về §1–§2
- [x] §4: AI log ≥ 3 hàng, có cột kiểm chứng
- [ ] Tự mở lại toàn bộ link trong §1 (bắt buộc trước khi nộp)
- [ ] Repo `Day16-Track1-2A202602499-LeThanhTung` đã tạo, chế độ Public, đã push memo.md
