---
id: collect-261121-veille-ia/go-json/gentoo-commits-repo-gentoo-master-commit-in-dev-libs-simdjson
title: "[gentoo-commits] repo/gentoo:master commit in: dev-libs/simdjson/"
domain: go-json
role: reference
task: article
actors: []
dates: ["2026-10-25"]
keywords: ["patrick", "mclean", "james", "gentoo", "dev-libs", "simdjson", "commit", "arthur"]
source: https://public-inbox.gentoo.org/gentoo-commits/1792965065.d447bd7eadcef0b5bbd37a865d427fb9ccc48212.sam@gentoo/
source_anchor: ""
source_lines: [1, 232]
section: "Go_JSON"
---
From: "Sam James" <sam@gentoo.org>
To: gentoo-commits@lists.gentoo.org
Subject: [gentoo-commits] repo/gentoo:master commit in: dev-libs/simdjson/
Date: Sun, 25 Oct 2026 21:52:23 +0000 (UTC)	[thread overview]
Message-ID: <1792965065.d447bd7eadcef0b5bbd37a865d427fb9ccc48212.sam@gentoo> (raw)
commit:     d447bd7eadcef0b5bbd37a865d427fb9ccc48212
Author:     Sam James <sam <AT> gentoo <DOT> org>
AuthorDate: Sun Oct 25 21:51:05 2026 +0000
Commit:     Sam James <sam <AT> gentoo <DOT> org>
CommitDate: Sun Oct 25 21:51:05 2026 +0000
URL:        https://gitweb.gentoo.org/repo/gentoo.git/commit/?id=d447bd7e
dev-libs/simdjson: Stabilize 5.0.3 arm, #984869
Signed-off-by: Sam James <sam <AT> gentoo.org>
 dev-libs/simdjson/simdjson-5.0.3.ebuild | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
diff --git a/dev-libs/simdjson/simdjson-5.0.3.ebuild b/dev-libs/simdjson/simdjson-5.0.3.ebuild
index 8baaf83d132b..d8d40cde34d8 100644
--- a/dev-libs/simdjson/simdjson-5.0.3.ebuild
+++ b/dev-libs/simdjson/simdjson-5.0.3.ebuild
@@ -24,7 +24,7 @@ SRC_URI="
 
 LICENSE="Apache-2.0 Boost-1.0 BSD MIT"
 SLOT="0/34"
-KEYWORDS="amd64 ~arm ~arm64 ~loong ~ppc64 ~riscv ~x86"
+KEYWORDS="amd64 arm ~arm64 ~loong ~ppc64 ~riscv ~x86"
 IUSE="+all-impls nan test tools"
 
 BDEPEND="


next             reply	other threads:[~2026-10-25 21:52 UTC|newest]
**Thread overview:** 173+ messages / expand[flat|nested]  mbox.gz  Atom feed  top
**2026-10-25 21:52 Sam James [this message]**
  -- strict thread matches above, loose matches on Subject: below --
2026-11-02 15:47 [gentoo-commits] repo/gentoo:master commit in: dev-libs/simdjson/ Patrick McLean
2026-10-29 14:13 Patrick McLean
2026-10-28 21:20 Patrick McLean
2026-10-27 14:49 Patrick McLean
2026-10-27 14:49 Patrick McLean
2026-10-25 21:52 Sam James
2026-10-25 21:52 Sam James
2026-10-25 21:52 Sam James
2026-10-25 21:52 Sam James
2026-10-15 15:06 Mike Pagano
2026-10-15  0:25 Patrick McLean
2026-10-13 18:37 Mike Pagano
2026-10-13 15:59 Patrick McLean
2026-10-11  1:57 Sam James
2026-10-11  1:12 Sam James
2026-10-11  1:08 Sam James
2026-10-09 18:29 Patrick McLean
2026-09-21 14:36 Patrick McLean
2026-09-03 16:54 Patrick McLean
2026-08-27 14:26 Patrick McLean
2026-08-14 21:31 Patrick McLean
2026-08-14 21:31 Patrick McLean
2026-07-20  0:25 Eli Schwartz
2026-07-19  7:38 Sam James
2026-07-19  7:27 Sam James
2026-06-10  1:28 Sam James
2026-06-10  1:28 Sam James
2026-06-10  1:28 Sam James
2026-06-10  1:28 Sam James
2026-04-17 21:37 Patrick McLean
2026-04-03 20:58 Patrick McLean
2026-04-01 16:31 Patrick McLean
2026-03-26 14:52 Patrick McLean
2026-03-21 22:14 Sam James
2026-03-20 16:47 Patrick McLean
2026-03-17 16:50 Patrick McLean
2026-03-16 14:32 Patrick McLean
2026-02-19 17:50 Patrick McLean
2026-02-19 17:50 Patrick McLean
2026-02-05 18:33 Arthur Zamarin
2026-02-05 18:33 Arthur Zamarin
2025-12-18 16:35 Patrick McLean
2025-12-18 16:35 Patrick McLean
2025-12-15 18:22 Patrick McLean
2025-12-12 23:00 Patrick McLean
2025-11-11 20:01 Patrick McLean
2025-11-04 19:29 Patrick McLean
2025-10-28 15:34 Patrick McLean
2025-09-30 17:49 Patrick McLean
2025-09-22 17:50 Patrick McLean
2025-09-19 15:46 Patrick McLean
2025-09-18  1:21 Patrick McLean
2025-09-16 14:39 Patrick McLean
2025-09-15 17:45 Patrick McLean
2025-09-12 23:32 Patrick McLean
2025-09-12 15:23 Patrick McLean
2025-09-12 15:23 Patrick McLean
2025-07-04 15:40 Arthur Zamarin
2025-07-04  9:42 Arthur Zamarin
2025-06-22 19:56 Arthur Zamarin
2025-06-22 15:23 Sam James
2025-06-22 14:34 Sam James
2025-06-04 18:09 Patrick McLean
2025-05-04 10:54 Sam James
2025-04-26 10:06 Arthur Zamarin
2025-04-26  5:48 Sam James
2025-04-26  2:56 Sam James
2025-04-26  1:45 Sam James
2025-04-26  1:25 Sam James
2025-02-12 18:32 Patrick McLean
2025-02-11 19:15 Patrick McLean
2025-01-09 17:54 Patrick McLean
2025-01-07 17:40 Patrick McLean
2025-01-07 17:40 Patrick McLean
2024-12-10 16:08 Patrick McLean
2024-12-06 18:42 Patrick McLean
2024-11-01 19:13 Andreas K. Hüttel
2024-10-31 13:51 Sam James
2024-10-30 15:46 Arthur Zamarin
2024-10-26 10:42 Sam James
2024-10-26 10:40 Sam James
2024-10-26 10:40 Sam James
2024-10-25 22:31 Sam James
2024-09-02 11:22 Jakov Smolić
2024-07-29 19:39 Arthur Zamarin
2024-07-29 19:37 Arthur Zamarin
2024-07-17  4:38 Viorel Munteanu
2024-06-26  5:43 Arthur Zamarin
2024-06-25 23:16 Sam James
2024-06-13 19:48 Patrick McLean
2024-05-30 18:41 Arthur Zamarin
2024-05-30 17:25 Patrick McLean
2024-05-30 17:25 Patrick McLean
2024-05-24 19:01 Patrick McLean
2024-05-24  2:46 Ionen Wolkens
2024-05-12 11:13 Yixun Lan
2024-04-05 20:50 Patrick McLean
2024-04-05 17:37 Patrick McLean
2024-04-05 17:36 Patrick McLean
2024-02-24  0:01 Patrick McLean
2024-02-07 18:38 Patrick McLean
2024-02-07 18:38 Patrick McLean
2024-02-07 18:38 Patrick McLean
2023-12-08 18:55 Patrick McLean
2023-12-04 20:25 Patrick McLean
2023-12-04 20:25 Patrick McLean
2023-10-28  2:01 Patrick McLean
2023-10-26 17:46 Patrick McLean
2023-09-20 16:54 Patrick McLean
2023-09-20 16:54 Patrick McLean
2023-08-22 18:11 Patrick McLean
2023-08-07 21:26 Patrick McLean
2023-07-07  6:07 Patrick McLean
2023-07-07  6:06 Patrick McLean
2023-07-07  6:06 Patrick McLean
2023-06-15 19:03 Patrick McLean
2023-06-15 19:03 Patrick McLean
2023-04-27 22:01 Patrick McLean
2023-03-14 17:33 Patrick McLean
2023-03-13 18:19 Patrick McLean
2023-03-13 18:19 Patrick McLean
2023-03-02 21:24 Patrick McLean
2023-03-02 21:24 Patrick McLean
2023-03-02 21:24 Patrick McLean
2023-02-14 21:28 Patrick McLean
2023-02-07 17:52 Patrick McLean
2023-01-26 18:44 Patrick McLean
2023-01-26 18:44 Patrick McLean
2023-01-13 10:16 Arthur Zamarin
2022-10-28 11:02 Sam James
2022-10-07 22:55 Patrick McLean
2022-08-01 19:03 Patrick McLean
2022-07-20  0:42 Patrick McLean
2022-06-30 21:35 Patrick McLean
2022-06-30 16:29 Patrick McLean
2022-06-30 16:29 Patrick McLean
2022-06-30 16:23 Patrick McLean
2022-06-16 16:01 Patrick McLean
2022-06-02 21:13 Patrick McLean
2022-06-02 16:02 Patrick McLean
2022-06-02 15:55 Patrick McLean
2022-05-26 21:21 Patrick McLean
2022-05-25 17:26 Patrick McLean
2022-05-18  1:22 Patrick McLean
2022-01-06 10:39 David Seifert
2021-10-28  0:55 Patrick McLean
2021-10-21 16:19 Patrick McLean
2021-10-05 20:57 Patrick McLean
2021-09-08 18:25 Patrick McLean
2021-09-08 18:25 Patrick McLean
2021-08-02 23:15 Patrick McLean
2021-06-07 18:19 Patrick McLean
2021-05-28  1:40 Patrick McLean
2021-05-22  7:42 Patrick McLean
2021-05-17  2:37 Sam James
2021-05-15 17:47 Patrick McLean
2021-04-16 22:06 Patrick McLean
2021-04-01 17:26 Patrick McLean
2021-04-01 17:26 Patrick McLean
2021-03-21  2:51 Patrick McLean
2021-02-11 17:41 Patrick McLean
2021-01-28 19:00 Patrick McLean
2021-01-28 19:00 Patrick McLean
2020-12-14 19:41 Patrick McLean
2020-12-07 18:20 Patrick McLean
2020-12-05 23:41 Patrick McLean
2020-12-05  0:34 Patrick McLean
2020-11-04 23:40 Patrick McLean
2020-10-30 17:33 Patrick McLean
2020-10-29  5:13 Sam James
2020-10-29  5:13 Sam James
2020-10-28 23:28 Georgy Yakovlev


**Reply instructions:**
You may reply publicly to this message via plain-text email
using any one of the following methods:
* Save the following mbox file, import it into your mail client,
  and reply-to-all from there: mbox
  Avoid top-posting and favor interleaved quoting:
  https://en.wikipedia.org/wiki/Posting_style#Interleaved_style
* Reply using the **--to**, **--cc**, and **--in-reply-to**
  switches of git-send-email(1):
  git send-email \
    --in-reply-to=1792965065.d447bd7eadcef0b5bbd37a865d427fb9ccc48212.sam@gentoo \
    --to=sam@gentoo.org \
    --cc=gentoo-commits@lists.gentoo.org \
    --cc=gentoo-dev@lists.gentoo.org \
    /path/to/YOUR_REPLY
  https://kernel.org/pub/software/scm/git/docs/git-send-email.html
* If your mail client supports setting the **In-Reply-To** header
  via mailto: links, try the mailto: link

  Be sure your reply has a **Subject:** header at the top and a blank line
  before the message body.

This is a public inbox, see mirroring instructions
for how to clone and mirror all data and code used for this inbox
