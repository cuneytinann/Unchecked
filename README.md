# Unchecked

[English](#english) | [Türkçe](#türkçe)

## English

A chess variant in a single HTML file. The game ends when a king is **captured**, not when it is checkmated. A move only has to be pseudo-legal: it must follow the normal movement rules, but you may leave your own king in check. Nobody tells you that you are in check. Spotting it is up to you.

Open `index.html` in a browser. No installation, server or dependencies are needed, and the file works as is on GitHub Pages. The interface is in English by default; Turkish can be selected from the menu in the top-left corner.

### Purpose

Unchecked is not meant to replace standard chess. Like antichess, atomic chess or three-check, it is a variant: the same board and pieces, with different rules. It has three goals:

- **Fewer draws, in favor of the stronger side.** Stalemate, insufficient material and dead positions save the side that is behind. Here they do not exist, so an advantage is easier to convert. A stalemated side has to move and loses its king, and a king with a single minor piece can still win. Draws remain possible through the 50-move rule and threefold repetition.
- **Punish illegal moves instead of preventing them.** Digital chess systems check every move and reject illegal ones, so a player never has to notice that their king is in check. Unchecked removes this check completely. Leaving your king in check is allowed; seeing it and capturing the king is up to the opponent. If the opponent does not see it, the game goes on. (Casual blitz already has a similar custom: capturing the king after an illegal move to claim the win.)
- **Measure attention, not only strategy, tactics and calculation.** In standard chess the rules keep watch for you: an illegal move cannot stand, and digital boards highlight a king in check. Here nobody keeps watch. Noticing your own king, and your opponent's, becomes part of playing strength.

### Rules

Pieces move as in standard chess: sliding pieces cannot jump over other pieces, you cannot capture your own pieces, and promotion and en passant work as usual. The only rule removed is the one that forbids leaving your king in check. You may move a pinned piece, move your king onto an attacked square, or ignore a check. These moves are not illegal here; they simply have a price, and your opponent is the one who collects it.

A game ends in one of three ways:

- **King capture.** Checkmate means the king will be captured on the next move. If that move is not played, the game goes on.
- **The 50-move rule.** If 100 plies (half-moves) pass with no pawn move and no capture, the game is drawn. It is applied automatically; no claim is needed.
- **Threefold repetition.** If the same position occurs for the third time, the game is drawn. This is also automatic.

The other endings of standard chess do not exist here:

- **No stalemate.** A side with no legal move under standard rules must still move, and every move puts its king where it can be captured.
- **No insufficient material, no dead position.** Since mate is not needed, king and knight, king and bishop, or even two bare kings are not drawn by themselves. A king and a single minor piece can build a stalemate, and a king that walks next to the enemy king gets captured. If nobody makes a mistake, the counters end the game.

**Castling.** The king and rook must not have moved, and the squares between them must be empty. Attacked squares are not checked: you may castle out of check, through an attacked square, or into check. In the last case your king can be captured on the next move.

**Repetition.** Two positions are the same if these four things match: the side to move, the piece placement, castling rights and the en passant right. Castling rights count as rights, even when castling is not possible at that moment. The en passant right counts only if an en passant capture is pseudo-legally possible in that position. This is the variant's version of the standard rule, which counts it only if the capture is legal.

**No moves at all.** A side with not even one pseudo-legal move cannot play, and the game is drawn. This only happens in very rare positions where a side is completely blocked by its own pieces.

### If added to a platform

This implementation has no clock and no draw offers. A platform that adds the variant would naturally add both, and a few rules need to be fixed:

- **Draw by agreement** can be allowed, as in standard chess. Apart from that, a game is drawn only by the 50-move rule or threefold repetition.
- **Running out of time always loses.** Article 6.9 of the FIDE Laws of Chess makes an exception: if the opponent cannot checkmate by any possible series of legal moves, the player whose time runs out is not lost, and the game is drawn. That exception does not fit here. Mate is not needed, and a king left next to the enemy king can always be captured, so there is almost always some series of moves that captures a king, even with bare kings. The exception would almost never apply and would only make the rules more complicated. We therefore propose that the side that runs out of time loses, whatever the material on the board.
- **Leaving the game loses** in the same way: a player who abandons the game, or disconnects and does not return in time, loses.

### Interface

The point of the variant is attention, so the board never helps the player:

- A king in check is not highlighted.
- The notation has no `+` or `#`.
- When you select a piece, its target squares are shown, but no capture marker is drawn on the enemy king's square. The move that captures the king can still be played.

When a game ends, tags appear in the move list:

- **missed:** the player could have captured the enemy king but didn't.
- **hanging:** after the move, the player's own king could be captured.
- **desperate:** the player was checkmated or stalemated under standard rules.
- **took the king:** the move that ended the game.

Tags are not shown during the game; that would be the board helping.

### Modes

**Warm-up.** A free game against the owl from the starting position. The owl's level can be chosen and changed mid-game; the new level applies from its next move.

**Two players.** Two people on one device. After each move the board stays still for about half a second, then turns to face the side to move. Undo takes back one ply, and Resign resigns for the side to move.

**Missions.** Unlocked when the first game against the owl ends or is resigned. Each mission has a starting position and a move limit. Every failed attempt reveals the next hint. When you succeed, you get an explanation, the solutions and an option to watch them.

**Presentation.** Never locked; open it from the start screen or the top menu. It walks through the variant's edge cases step by step. In the presentation, a king that the side to move can capture is marked with a red ring; the game itself never shows this. The half-move counter and the repetition count are shown at every step. Use the left and right arrow keys to move through it. When you leave the presentation, an unfinished game continues where it stopped.

### Presentation scenes

1. **Mate, one move later.** Scholar's mate. After being mated, Black still has to move, and its king is captured on the next move.
2. **No stalemate.** A standard stalemate with king and queen. Black has to move, and its king is captured.
3. **No dead positions.** King and bishop against king. The 50-move counter ends the game (the scene starts the counter at 96).
4. **Bare kings and threefold repetition.** The kings shuffle back and forth until the starting position occurs a third time. The details of position comparison are explained here.
5. **The unnoticed mate.** White mates without realizing it. Black gives a desperate check, White defends instead of capturing the king, and in the end White is the one mated and loses.
6. **Spot the illegal move, win.** White moves a pinned knight; Black notices and captures the king.
7. **Miss the illegal move, miss the win.** Same opening, same mistake. Black captures the knight instead of the king, White blocks with a pawn, and the game goes on.

### The owl

The owl plays by standard rules and has never heard of this variant. On every move it follows this order:

1. If it can capture the enemy king, it does.
2. Otherwise it picks the best move among those that are legal under standard rules. It never leaves its own king in check.
3. If it has no legal move (checkmate or stalemate under standard rules), it captures the most valuable piece it can. If it cannot capture anything, it plays the first move found while scanning the board from a8 to h1. This is the moment its king is left hanging.

The search is negamax with alpha-beta pruning, followed by a quiescence search over captures. The evaluation is material plus simple piece-square tables. The owl's beliefs are those of standard chess: in its search, stalemate is a draw (score 0). So when it is losing it does not avoid stalemate, and even sees it as a way out. The stalemate mission is built on this.

| Level | Search |
|---|---|
| Easy | 1 ply, no quiescence search |
| Medium | 2 plies with quiescence search |
| Hard | 2 plies, then 3 plies with a 40,000-node limit |
| Master | Hard, then 4 plies with a 120,000-node limit |

If a depth hits its node limit, it is discarded and the move from the last completed depth is played. The limits count nodes, not time, so the owl plays the same move in the same position on every device. In missions the owl always plays on **Hard**; the solutions were verified at this level.

### Missions

| # | Mission | Limit | What it shows | Solutions |
|---|---|---|---|---|
| 1 | One more move | 2 | Capturing the king after mate | 1.Re8 Qxc2 2.Rxg8 · 1.Qc8 Qxf2 2.Qxg8 |
| 2 | Take first | 1 | Capturing first while in check | 1…Bxd4 2.Rxg8 |
| 3 | Silent check | 3 | The price of mating while your own king hangs | 1.gxf3 Kg8 2.Qxg7 Kxg7 3.Bxg7 · 1.gxf3 Kg8 2.Qe8 Kf8 3.Qxf8 |
| 4 | The pinned bishop | 1 | Capturing the king with a pinned piece | 1…Kxg7 2.Bxg7 |
| 5 | Castling under fire | 2 | Castling out of check | 1.O-O-O Kc8 2.Qxc8 |
| 6 | Stalemate loses | 5 | Winning a textbook rook-pawn draw | 1.Kg5 Kf7 2.h6 Kg8 3.Kg6 Kh8 4.h7 Kxh7 5.Kxh7 |
| 7 | A lone knight | 4 | Winning with king and knight | 1.Kf7 Kh7 2.Ng4 Kh8 3.Nf6 Kg8 4.Kxg8 · 1.Nf5 Kh7 2.Ne7 Kh8 3.Kg6 Kg8 4.Nxg8 |

Starting positions (FEN):

```
1  6k1/5ppp/8/8/8/8/q1Q2PPP/4R1K1 w - - 0 1
2  4R1k1/5ppp/1b6/8/3Q4/8/6PP/6K1 b - - 0 1
3  7k/pp4pp/8/4Q3/8/2B2n2/PP3PPP/6K1 w - - 0 1
4  2r4k/pp4Qp/8/8/8/2B5/PP6/2K5 b - - 0 1
5  3k3r/6pp/1P2Q3/8/8/8/6n1/R3K3 w Q - 0 1
6  8/6k1/8/5K1P/8/8/8/8 w - - 0 1
7  7k/8/5K2/8/8/4N3/8/8 w - - 0 1
```

In missions 2 and 4 Black moves first. The owl plays that move, and the limit counts only White's moves.

**Verification.** For every mission, a full search was run up to the move limit over all of White's pseudo-legal moves, with the owl's deterministic replies. The table lists every solution within the limit, and no mission has a shorter solution. Mission 7 has three solution lines: two of them share the first three moves and end with either the king or the knight capturing.

### Technical notes

Everything is in one HTML file, in these sections (section headers and code comments are in Turkish):

- **MOTOR (engine):** move generation, variant and standard castling, notation, the repetition key and the owl's search.
- **GÖREVLER, DİL (missions, text):** mission data and all English and Turkish text.
- **SUNUM (presentation):** scene data and captions. The presentation's moves are scripted for both sides; the owl is not used.
- **UYGULAMA (app):** game flow, rendering, input and dialogs.

The move generator was checked with perft: 197,281 nodes at depth 4 from the starting position, and 97,862 nodes at depth 3 from the "Kiwipete" position.

The only external resource is the Alegreya typeface from Google Fonts. Without an internet connection the page falls back to the system serif font and works the same. No browser storage is used; nothing is saved.

The structure follows [ParrotPuzzle](https://github.com/cuneytinann/fidelite/blob/main/extra/ParrotPuzzle.html): a warm-up first, then missions that unlock, step-by-step hints, an explanation and a solution replay.

---

## Türkçe

Tek bir HTML dosyasında bir satranç varyantı. Oyun mat ile değil, şahın **yenmesiyle** biter. Bir hamlenin yalnızca pseudo-legal olması yeterlidir: taşların normal hareket kurallarına uymalıdır, ama kendi şahını şah altında bırakabilirsin. Kimse sana şah çekildiğini söylemez. Görmek sana kalmış.

`index.html` dosyasını bir tarayıcıda açman yeterli. Kurulum, sunucu ya da bağımlılık gerekmez ve dosya GitHub Pages'te olduğu gibi çalışır. Arayüz varsayılan olarak İngilizcedir; Türkçe sol üst köşedeki menüden seçilebilir.

### Amaç

Unchecked standart satrancın yerine geçmek için yapılmadı. Antichess, atomic ya da three-check gibi bir varyant: aynı tahta ve taşlar, farklı kurallar. Üç amacı var:

- **Beraberliği azaltmak, üstün olan tarafın lehine.** Pat, yetersiz materyal ve ölü pozisyon geride olan tarafı kurtarır. Burada bunlar olmadığı için üstünlüğü kazanca çevirmek daha kolaydır. Pat olan taraf oynamak zorunda kalır ve şahını kaybeder; şah ve tek bir hafif taş bile kazanabilir. Beraberlik yine 50 hamle kuralı ve üç kez tekrarla mümkündür.
- **Kural dışı hamleyi engellemek yerine cezalandırmak.** Dijital satranç sistemleri her hamleyi kontrol eder ve kural dışı hamleyi kabul etmez; oyuncu şahının şah altında olduğunu fark etmek zorunda kalmaz. Unchecked bu kontrolü tamamen kaldırır. Şahını şah altında bırakmak serbesttir; bunu görüp şahı almak rakibe kalmıştır. Rakip görmezse oyun sürer. (Gündelik blitz oyunlarında benzer bir alışkanlık zaten var: kural dışı hamleden sonra şahı alıp galibiyeti istemek.)
- **Yalnızca stratejiyi, taktiği ve hesabı değil, dikkati de ölçmek.** Standart satrançta kurallar senin yerine nöbet tutar: kural dışı hamle geçerli sayılmaz, dijital tahtalar şah altındaki şahı vurgular. Burada nöbet tutan kimse yok. Kendi şahını ve rakibinkini görmek oyun gücünün bir parçası olur.

### Kurallar

Taşlar standart satrançtaki gibi hareket eder: uzun menzilli taşlar başka taşların üzerinden atlayamaz, kendi taşını alamazsın, terfi ve geçerken alma her zamanki gibidir. Kaldırılan tek kural, şahını şah altında bırakmayı yasaklayan kuraldır. Açmazdaki taşı oynayabilir, şahını tehdit edilen bir kareye sürebilir ya da çekilen şahı görmezden gelebilirsin. Bu hamleler burada kural dışı değildir; yalnızca bir bedeli vardır ve bu bedeli ödetecek olan rakiptir.

Oyun üç yoldan biriyle biter:

- **Şahın yenmesi.** Mat, şahın bir sonraki hamlede yeneceği anlamına gelir. O hamle oynanmazsa oyun sürer.
- **50 hamle kuralı.** Piyon oynamadan ve taş alınmadan 100 yarım hamle geçerse oyun berabere biter. Otomatik uygulanır; talep gerekmez.
- **Üç kez tekrar.** Aynı pozisyon üçüncü kez oluşursa oyun berabere biter. Bu da otomatiktir.

Standart satrançtaki öbür bitişler burada yoktur:

- **Pat yok.** Standart kurallara göre yasal hamlesi olmayan taraf yine de oynamak zorundadır ve her hamlesi şahını yenebilecek bir kareye götürür.
- **Yetersiz materyal ve ölü pozisyon yok.** Mat gerekmediği için şah ve at, şah ve fil, hatta iki çıplak şah bile kendiliğinden berabere sayılmaz. Şah ve tek bir hafif taşla pat kurulabilir, rakip şahın yanına giden şah da yenir. Kimse hata yapmazsa oyunu sayaçlar bitirir.

**Rok.** Şah ve kale oynamamış olmalı, aralarındaki kareler boş olmalıdır. Tehdit altındaki karelere bakılmaz: şah altındayken, tehdit edilen bir kareden geçerek ya da şah altına rok yapabilirsin. Son durumda şahın bir sonraki hamlede yenebilir.

**Tekrar.** İki pozisyon şu dört şey aynıysa aynı sayılır: sıranın kimde olduğu, taşların yerleşimi, rok hakları ve geçerken alma hakkı. Rok hakları, o an rok yapılamasa bile hak olarak sayılır. Geçerken alma hakkı ise yalnızca o pozisyonda pseudo-legal bir geçerken alma mümkünse sayılır. Bu, yalnızca yasal olarak alınabiliyorsa sayan standart kuralın bu varyanttaki karşılığıdır.

**Hiç hamle yoksa.** Tek bir pseudo-legal hamlesi bile olmayan taraf oynayamaz ve oyun berabere biter. Bu yalnızca bir tarafın kendi taşlarıyla tamamen kilitlendiği çok nadir pozisyonlarda olur.

### Platformlara eklenirse

Bu uygulamada saat ve beraberlik teklifi yok. Varyantı ekleyen bir platform doğal olarak ikisini de ekler; bu durumda birkaç kuralın netleşmesi gerekir:

- **Anlaşmalı beraberlik** standart satrançtaki gibi serbest bırakılabilir. Bunun dışında oyun yalnızca 50 hamle kuralı ya da üç kez tekrarla berabere biter.
- **Süresi biten her zaman kaybeder.** FIDE Satranç Kuralları'nın 6.9 maddesi bir istisna tanır: rakip, yasal hamlelerin hiçbir olası dizisiyle mat edemiyorsa süresi biten oyuncu kaybetmez ve oyun berabere biter. Bu istisna burada uymaz. Mat gerekmez ve rakip şahın yanında bırakılan şah her zaman yenebilir; bu yüzden çıplak şahlarda bile bir şahı alan bir hamle dizisi neredeyse her zaman vardır. İstisna hemen hiç uygulanmaz, yalnızca kuralları karmaşıklaştırır. Bu yüzden önerimiz şu: tahtadaki materyal ne olursa olsun, süresi biten taraf kaybeder.
- **Oyundan çekilen de kaybeder:** oyunu terk eden ya da bağlantısı kopup süresi içinde geri dönmeyen oyuncu kaybeder.

### Arayüz

Varyantın asıl konusu dikkat, bu yüzden tahta oyuncuya hiçbir zaman yardım etmez:

- Şah altındaki şah vurgulanmaz.
- Notasyonda `+` ya da `#` yoktur.
- Bir taş seçtiğinde gidebileceği kareler gösterilir, ama rakip şahın karesine yeme işareti konmaz. Şahı alan hamle yine de oynanabilir.

Oyun bittiğinde hamle listesinde etiketler çıkar:

- **kaçırıldı:** oyuncu rakip şahı alabilecekken almadı.
- **açıkta:** hamleden sonra oyuncunun kendi şahı yenebilir durumda kaldı.
- **çaresiz:** oyuncu standart kurallara göre mat ya da pattaydı.
- **şahı aldı:** oyunu bitiren hamle.

Etiketler oyun sırasında gösterilmez; gösterilseydi tahta yardım etmiş olurdu.

### Modlar

**Isınma.** Başlangıç pozisyonundan baykuşa karşı serbest oyun. Baykuşun seviyesi seçilebilir ve oyunun ortasında değiştirilebilir; yeni seviye bir sonraki hamlesinden itibaren geçerli olur.

**İki oyuncu.** Aynı cihazda iki kişi. Her hamleden sonra tahta yarım saniye kadar olduğu gibi kalır, sonra sırası gelen tarafa döner. "Geri al" bir yarım hamleyi geri alır, "Pes et" sırası gelen taraf adına pes eder.

**Görevler.** Baykuşa karşı ilk oyun bittiğinde ya da pes edildiğinde açılır. Her görevin bir başlangıç pozisyonu ve hamle sınırı vardır. Başarısız her denemede bir sonraki ipucu açılır. Başarınca bir açıklama, çözümler ve onları izleme seçeneği gelir.

**Sunum.** Hiçbir zaman kilitli değildir; açılış ekranından ya da üst menüden açılır. Varyantın uç durumlarını adım adım gösterir. Sunumda, sırası gelen tarafın alabileceği şah kırmızı bir halkayla işaretlenir; oyunun kendisi bunu hiçbir zaman göstermez. Yarım hamle sayacı ve tekrar sayısı her adımda görünür. Sol ve sağ ok tuşlarıyla ilerlenebilir. Sunumdan çıkınca yarım kalan oyun kaldığı yerden devam eder.

### Sunum sahneleri

1. **Mat, bir hamle sonra.** Çoban matı. Mat olduktan sonra da siyah oynamak zorundadır ve şahı bir sonraki hamlede yenir.
2. **Pat yok.** Şah ve vezirle standart bir pat. Siyah oynamak zorundadır ve şahı yenir.
3. **Ölü pozisyon yok.** Şah ve fil, şaha karşı. Oyunu 50 hamle sayacı bitirir (sahne sayacı 96'dan başlatır).
4. **Çıplak şahlar ve üç kez tekrar.** Şahlar gidip gelir ve başlangıç pozisyonu üçüncü kez oluşur. Pozisyon karşılaştırmasının ayrıntıları burada anlatılır.
5. **Fark edilmeyen mat.** Beyaz farkında olmadan mat eder. Siyah çaresizce şah çeker, beyaz şahı almak yerine savunur ve sonunda mat olan da kaybeden de beyaz olur.
6. **Kural dışını gören kazanır.** Beyaz açmazdaki atı oynar; siyah fark eder ve şahı alır.
7. **Kural dışını görmeyen kaçırır.** Aynı açılış, aynı hata. Siyah şah yerine atı alır, beyaz araya piyon sokar ve oyun sürer.

### Baykuş

Baykuş standart kurallarla oynar ve bu varyantı hiç duymamıştır. Her hamlesinde şu sırayı izler:

1. Rakip şahı alabiliyorsa alır.
2. Alamıyorsa standart kurallara göre yasal hamleleri arasından en iyisini seçer. Kendi şahını hiçbir zaman şah altında bırakmaz.
3. Yasal hamlesi yoksa (standart kurallara göre mat ya da patsa) alabildiği en değerli taşı alır. Hiçbir şey alamıyorsa tahtayı a8'den h1'e tararken bulduğu ilk hamleyi oynar. Şahının açıkta kaldığı an budur.

Arama, alfa-beta budamalı negamax'tır; ardından yalnızca alışverişlere bakan bir quiescence search gelir. Değerlendirme, materyal artı basit piece-square tablolarından oluşur. Baykuşun inançları standart satrancınkilerdir: aramasında pat beraberliktir (puan 0). Bu yüzden kaybederken pattan kaçmaz, hatta onu bir kurtuluş olarak görür. Pat görevi bunun üzerine kuruludur.

| Seviye | Arama |
|---|---|
| Kolay | 1 yarım hamle, quiescence search yok |
| Orta | quiescence search ile 2 yarım hamle |
| Zor | 2 yarım hamle, ardından 40.000 düğüm sınırıyla 3 yarım hamle |
| Usta | Zor, ardından 120.000 düğüm sınırıyla 4 yarım hamle |

Bir derinlik düğüm sınırına takılırsa o derinlik atılır ve son tamamlanan derinliğin hamlesi oynanır. Sınırlar süreyi değil düğümleri sayar, bu yüzden baykuş aynı pozisyonda her cihazda aynı hamleyi oynar. Görevlerde baykuş hep **Zor** seviyede oynar; çözümler bu seviyede doğrulandı.

### Görevler

Notasyon Türkçe taş harfleriyle yazılmıştır: Ş şah, V vezir, K kale, F fil, A at.

| # | Görev | Sınır | Gösterdiği | Çözümler |
|---|---|---|---|---|
| 1 | Bir hamle daha | 2 | Mattan sonra şahı almak | 1.Ke8 Vxc2 2.Kxg8 · 1.Vc8 Vxf2 2.Vxg8 |
| 2 | Önce sen al | 1 | Şah altındayken önce şahı almak | 1…Fxd4 2.Kxg8 |
| 3 | Sessiz şah | 3 | Kendi şahın açıktayken mat etmenin bedeli | 1.gxf3 Şg8 2.Vxg7 Şxg7 3.Fxg7 · 1.gxf3 Şg8 2.Ve8 Şf8 3.Vxf8 |
| 4 | Açmazdaki fil | 1 | Açmazdaki taşla şahı almak | 1…Şxg7 2.Fxg7 |
| 5 | Ateş altında rok | 2 | Şah altındayken rok | 1.O-O-O Şc8 2.Vxc8 |
| 6 | Pat kaybettirir | 5 | Kale piyonlu teorik beraberliği kazanmak | 1.Şg5 Şf7 2.h6 Şg8 3.Şg6 Şh8 4.h7 Şxh7 5.Şxh7 |
| 7 | Tek at | 4 | Şah ve atla kazanmak | 1.Şf7 Şh7 2.Ag4 Şh8 3.Af6 Şg8 4.Şxg8 · 1.Af5 Şh7 2.Ae7 Şh8 3.Şg6 Şg8 4.Axg8 |

Başlangıç pozisyonları (FEN):

```
1  6k1/5ppp/8/8/8/8/q1Q2PPP/4R1K1 w - - 0 1
2  4R1k1/5ppp/1b6/8/3Q4/8/6PP/6K1 b - - 0 1
3  7k/pp4pp/8/4Q3/8/2B2n2/PP3PPP/6K1 w - - 0 1
4  2r4k/pp4Qp/8/8/8/2B5/PP6/2K5 b - - 0 1
5  3k3r/6pp/1P2Q3/8/8/8/6n1/R3K3 w Q - 0 1
6  8/6k1/8/5K1P/8/8/8/8 w - - 0 1
7  7k/8/5K2/8/8/4N3/8/8 w - - 0 1
```

Görev 2 ve 4'te ilk hamle siyahındır. O hamleyi baykuş oynar ve sınır yalnızca beyazın hamlelerini sayar.

**Doğrulama.** Her görev için, beyazın bütün pseudo-legal hamleleri ve baykuşun deterministik cevaplarıyla hamle sınırına kadar tam arama yapıldı. Tablo, sınır içindeki bütün çözümleri listeler ve hiçbir görevin daha kısa bir çözümü yoktur. Görev 7'de üç çözüm dizisi vardır: ikisi ilk üç hamleyi paylaşır ve şahın ya da atın almasıyla biter.

### Teknik notlar

Her şey tek bir HTML dosyasında, şu bölümlerde (bölüm başlıkları ve kod yorumları Türkçedir):

- **MOTOR:** hamle üretimi, varyant ve standart rok, notasyon, tekrar anahtarı ve baykuşun araması.
- **GÖREVLER, DİL:** görev verileri ile bütün İngilizce ve Türkçe metinler.
- **SUNUM:** sahne verileri ve açıklamaları. Sunumdaki hamleler iki taraf için de elle yazılmıştır; baykuş kullanılmaz.
- **UYGULAMA:** oyun akışı, çizim, girdi ve diyaloglar.

Hamle üreticisi perft ile kontrol edildi: başlangıç pozisyonundan 4 derinlikte 197.281 düğüm, "Kiwipete" pozisyonundan 3 derinlikte 97.862 düğüm.

Tek dış kaynak Google Fonts'tan yüklenen Alegreya yazı tipidir. İnternet bağlantısı yoksa sayfa sistemin serif yazı tipine geçer ve aynı şekilde çalışır. Tarayıcı depolaması kullanılmaz; hiçbir şey kaydedilmez.

Yapı [ParrotPuzzle](https://github.com/cuneytinann/fidelite/blob/main/extra/ParrotPuzzle.html)'ı izler: önce ısınma, sonra kilidi açılan görevler, adım adım ipuçları, bir açıklama ve çözümün tekrar oynatılması.
