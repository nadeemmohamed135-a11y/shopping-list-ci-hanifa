## Eigene �berlegungen und Verst�ndnis

Beim Aufbau der CI/CD-Pipeline musste ich einige Schritte und Entscheidungen treffen. Ziel war es, eine automatisierte und wartungsfreundliche Auslieferung der Anwendung zu erm�glichen.

Besondere Aufmerksamkeit galt der Aufteilung der Pipeline in einzelne Jobs (Lint, Test, Deployment) und der Sicherstellung, dass Codequalit�t und Funktionalit�t laufend gepr�ft werden.
Ich hatte ein paar npm Probleme die ich mit hilfe von AI aber l�sen konnte

---

## �berlegungen zum Aufbau

- Linting-Job sollte sofort nach jedem Push den Code-Stil �berpr�fen, damit Fehler fr�h erkannt werden und nicht unn�tig in die weiteren Schritte gelangen.
- Testing-Job pr�ft die Funktionalit�t mit Unit- und Komponenten-Tests (hier mit Jest und Testing Library). Dieser Schritt verhindert, dass fehlerhafte �nderungen deployed werden.
- Mithilfe von `needs:` wurde eine logische Abfolge im Workflow geschaffen ? deployment l�uft nur bei Erfolg von Linting und Testing.
- Die Installation der Dependencies ist in jedem Job notwendig, da die Jobs separat laufen und ein eigenes Environment haben.

---

## Probleme und L�sungen

- Beim Einrichten der Pipeline gab es Probleme mit Peer-Dependencies, insbesondere wegen der React-19-Vorabversion. Dies konnte durch die Option `--legacy-peer-deps` bei der Installation gel�st werden.
- Ein h�ufiger Fehler war, dass nach der Jobs-Trennung das `next`-CLI nicht gefunden wurde. Die L�sung war, in jedem Job die Dependencies erneut zu installieren und immer die aktuelle `package-lock.json` zu committen.
- Die Trennung in verschiedene Jobs hat geholfen, Fehler schneller und gezielter zu finden.

---

## Kritik und Verbesserungsvorschl�ge

- Zuk�nftig sollte die Pipeline um einen echten Deployment-Schritt erg�nzt werden, zum Beispiel mit Vercel oder Netlify.
- Weitere Automatisierung, z.B. f�r Pull Requests, k�nnte die Code-Qualit�t noch besser absichern.
- Die Testing-Abdeckung k�nnte ausgebaut werden, sodass auch End-to-End-Tests automatisiert laufen.
