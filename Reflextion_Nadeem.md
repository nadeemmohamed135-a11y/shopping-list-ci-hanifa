## Eigene Überlegungen und Verständnis

Beim Aufbau der CI/CD-Pipeline musste ich einige Schritte und Entscheidungen treffen. Ziel war es, eine automatisierte und wartungsfreundliche Auslieferung der Anwendung zu ermöglichen.

Besondere Aufmerksamkeit galt der Aufteilung der Pipeline in einzelne Jobs (Lint, Test, Deployment) und der Sicherstellung, dass Codequalität und Funktionalität laufend geprüft werden.
Ich hatte ein paar npm Probleme die ich mit hilfe von AI aber lösen konnte

---

## Überlegungen zum Aufbau

- Linting-Job sollte sofort nach jedem Push den Code-Stil überprüfen, damit Fehler früh erkannt werden und nicht unnötig in die weiteren Schritte gelangen.
- Testing-Job prüft die Funktionalität mit Unit- und Komponenten-Tests (hier mit Jest und Testing Library). Dieser Schritt verhindert, dass fehlerhafte Änderungen deployed werden.
- Mithilfe von `needs:` wurde eine logische Abfolge im Workflow geschaffen ? deployment läuft nur bei Erfolg von Linting und Testing.
- Die Installation der Dependencies ist in jedem Job notwendig, da die Jobs separat laufen und ein eigenes Environment haben.

---

## Probleme und Lösungen

- Beim Einrichten der Pipeline gab es Probleme mit Peer-Dependencies, insbesondere wegen der React-19-Vorabversion. Dies konnte durch die Option `--legacy-peer-deps` bei der Installation gelöst werden.
- Ein häufiger Fehler war, dass nach der Jobs-Trennung das `next`-CLI nicht gefunden wurde. Die Lösung war, in jedem Job die Dependencies erneut zu installieren und immer die aktuelle `package-lock.json` zu committen.
- Die Trennung in verschiedene Jobs hat geholfen, Fehler schneller und gezielter zu finden.

---

## Kritik und Verbesserungsvorschläge

- Zukünftig sollte die Pipeline um einen echten Deployment-Schritt ergänzt werden, zum Beispiel mit Vercel oder Netlify.
- Weitere Automatisierung, z.B. für Pull Requests, könnte die Code-Qualität noch besser absichern.
- Die Testing-Abdeckung könnte ausgebaut werden, sodass auch End-to-End-Tests automatisiert laufen.
