# Stattup logok elemzése

### loggingMode: file
Mivel az alkalmazás ekkor nem a standard outputra logol, így a  Spring boot logon kívül más nem jelenik meg a naplóban.
![nativ file startup logs](./screenshots/startup_nativ_file.PNG "Nativ file: indulási logok")
Filter, gyakorlatilag a fenti képen is látható 3 mezőre van értelme beállítani. De mivel az alkalmazás logok nem jellenek meg ennek nem sok gyakorlati haszna van - legfeljebb a többi alkalmazás vagy alkalmazás példánytól való elkülönítésére használhatjuk.
![nativ file possible filters](./screenshots/filter_nativ_file.PNG "Nativ file: lehetséges filterezés")

### loggingMode: file-json
Ekkor a Spring boot banner előtt megjelenik egy sor másik log is, de érdekességük, hogy a formátumok a beállítás ellenére nem JSON. 
![nativ file-json startup logs](./screenshots/startup_nativ_file-json.PNG "Nativ file-json: indulási logok")
Szűrni itt is legfeljebb a 3 mezőre van értelme. A megjelenet logok, és a Spring banner logjainak elkülönítésére nincs érdemi módom (legfeljebb a line contetntnél ki tudom használni, hogy a Spring boot logjai nem számmal kezdődnek):
![nativ file-json posible filters](./screenshots/filter_nativ_file-json.PNG "Nativ file-json: lehetséges filterezés")
Illetve magukra megjelenő logok sorainak tartalmára is szűrhetek hasonlóan.
Ugyanakkor a működés közben az alkalmazás ekkor sem logol semmit, hiszen nem a standard outputra logol.

### loggingMode: stdout
Ekkor a banner felett megjelenik az alkalmazás startup logja is.
![nativ stdout startup logs](./screenshots/startup_nativ_stdout.PNG "Nativ stdout: indulási logok")
De mivel az alkalmazás nem json-ben logol, így érdemi (label alapú) filterezésem itt is legfeljebb arra a bizonyos 3 mezőre van. Ekkor a standard output miatt megjelenek a működési logok is, de mivel a formátumuk ezeknek sem strukturált (json), így nagyon korlátozottan tudok rájuk filterezni, leginkább a konkrét üzenet tatrtalmak alapján a különféle Line operátorokkal. Az alábbi logrészleten látható, hogy a stacktracek például több log üzenetre törnek, melyek összetartozó leválogatása például lehetetlen lenne:
![nativ stdout work logs](./screenshots/work_nativ_stdout.PNG "Nativ stdout: munka logok")

### loggingMode: stdout-json
Ebben az esetben a Spring boot banner-t leszámítva minden log strukturált json formában jelenik meg:
![nativ stdout-json startup logs](./screenshots/startup_nativ_stdout-json.PNG "Nativ stdout-json: indulási logok")

A label alapú filterezési lehetőségeim így kinyílnak. Az alábbi képen aláhúztam ezeket:
![nativ stdout-json new labels](./screenshots/new_labels_nativ_stdout-json.PNG "Nativ stdout-json: új címkék")
Ezekre a label-ek szintjén tudok szűrni.
Szintén megjelennek a működéi logok, struktúrált formában:
![nativ stdout-json work logs](./screenshots/work_nativ_stdout-json.PNG "Nativ stdout-json: működési logok")
Ezeket el tudom egymástól különíteni a thread_name label alapján egymástól: 
![nativ stdout-json work logs separate filter](./screenshots/filter_separate_worklogs.PNG "Nativ stdout-json: működési logok és a startup logok elválasztása")

De arra is van lehetőségem, hogy szűrjek a logok szintjére: 
![stdout-json log level separate filter](./screenshots/filter_separate_loglevels.PNG "stdout-json: logszintek szétválasztása")

Látható továbbá, hogy a hibáknál a verem kivonat szintén egy elkülönült field-be kerül:
![stdout-json stacktrace](./screenshots/stacktrace.PNG "stdout-json: verem kivonat a stack_trace mezőben")
 
Fent leírtakban a neativ helyett a jvm image-et használva nem láttam lényegi különbséget. Ezért mindkettőre igaz, hogy a **stdout-json** a megfelelő választás.

A két képfájlban startup logjait így tudjuk leszűrni:
![stdout-json filter setted logger profile](./screenshots/filter_profile.PNG "stdout-json:szűrés a log profilokat mutató startup üzenetre")
**Itt a lényeges különbség a kétféle image közt, hogy a nativ image indulási ideje 12 másodperc, míg a jvm-é 200 msec.
Ezért a natív alkalmasabb a felhőhöz (rövidebb az indulási ideje.)**

# Kérések logolásai
## A használt szűrő
![stdout-json logger profiles](./screenshots/filter_worklogs.PNG "Munkalogok szűrése (export spans hibák nélkül)")
## Logok:
![worklogs](./screenshots/worklogs.PNG "Formázott, szűrt munkalogok")
