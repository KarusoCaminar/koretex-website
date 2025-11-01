# 🔧 KOMPLETTE n8n Konfigurationsanleitung

## Übersicht

Diese Anleitung führt dich Schritt für Schritt durch alle manuellen Konfigurationen, die nach dem Import der Workflow JSON in n8n durchgeführt werden müssen.

**Referenz:** Basierend auf offizieller n8n-Dokumentation: https://docs.n8n.io/

---

## 📋 Inhalt

1. [Workflow Import](#1-workflow-import)
2. [Google Vertex AI Credentials](#2-google-vertex-ai-credentials)
3. [Chat Model Node Konfiguration](#3-chat-model-node-konfiguration)
4. [AI Agent Node Konfiguration](#4-ai-agent-node-konfiguration)
5. [Webhook Node Konfiguration](#5-webhook-node-konfiguration)
6. [IF Nodes Verifikation](#6-if-nodes-verifikation)
7. [Workflow Aktivierung und Test](#7-workflow-aktivierung-und-test)

---

## 1. Workflow Import

### Schritt 1.1: Workflow JSON importieren

1. **Öffne n8n:** Gehe zu deiner n8n-Instanz (z.B. `https://n8n2.kortex-system.de`)
2. **Workflows öffnen:** Klicke auf **"Workflows"** im Hauptmenü
3. **Import starten:** Klicke auf **"Import from File"** oder **"Import"**
4. **JSON-Datei auswählen:** Wähle die Datei `n8n-business-card-workflow-vertex-COMPLETE.json`
5. **Import bestätigen:** Klicke auf **"Import"**

✅ **Erwartetes Ergebnis:** Der Workflow wird importiert und alle Nodes sind sichtbar.

⚠️ **WICHTIG:** Credentials werden beim Import NICHT übertragen. Diese müssen manuell konfiguriert werden!

---

## 2. Google Vertex AI Credentials

### Schritt 2.1: Google Service Account JSON vorbereiten

Du benötigst eine Google Service Account JSON-Datei mit folgenden Informationen:
- `project_id`: Dein Google Cloud Project ID (z.B. `koretex-zugang`)
- `private_key`: Private Key aus der JSON-Datei
- `client_email`: Service Account E-Mail (z.B. `service-account@project.iam.gserviceaccount.com`)

### Schritt 2.2: Google API Credentials in n8n erstellen

1. **Credentials öffnen:** In n8n, gehe zu **"Credentials"** im Hauptmenü (oder klicke auf "Credentials" in einem Node)
2. **Neue Credentials erstellen:** Klicke auf **"Create Credential"** oder **"+"**
3. **Credential-Typ wählen:** Suche und wähle **"Google API"** oder **"Google Service Account"**
4. **Credential konfigurieren:**
   - **Name:** `Google Service Account Moritz` (oder ein anderer Name)
   - **Service Account Email:** Kopiere aus deiner Service Account JSON (`client_email`)
   - **Private Key:** Kopiere den vollständigen `private_key` aus deiner JSON (inklusive `-----BEGIN PRIVATE KEY-----` und `-----END PRIVATE KEY-----`)
   - **Project ID:** Dein Google Cloud Project ID (z.B. `koretex-zugang`)
5. **Speichern:** Klicke auf **"Save"** oder **"Create"**

✅ **Erwartetes Ergebnis:** Die Credentials werden erstellt und können in Nodes verwendet werden.

📖 **Referenz:** https://docs.n8n.io/credentials/google/

---

## 3. Chat Model Node Konfiguration

### Schritt 3.1: Google Vertex Chat Model Node öffnen

1. **Node finden:** Im Workflow findest du den Node **"Google Vertex Chat Model"** (oben links, isoliert)
2. **Node öffnen:** Klicke auf den Node

### Schritt 3.2: Chat Model konfigurieren

1. **Parameters Tab öffnen:** Stelle sicher, dass der **"Parameters"** Tab aktiv ist

2. **Project ID prüfen:**
   - **Project ID:** Sollte `koretex-zugang` sein (oder dein eigenes Project ID)
   - Falls leer: Tippe dein Google Cloud Project ID ein

3. **Model auswählen:**
   - **Model:** Empfohlene Optionen:
     - ✅ `gemini-2.5-flash` (Schnell & gute Qualität - **EMPFOHLEN**)
     - `gemini-2.5-pro` (Beste Qualität, langsamer)
     - `gemini-1.5-pro` (Ältere Version)
     - `gemini-1.5-flash` (Schneller, ältere Version)

4. **Location auswählen:**
   - **Location:** Wähle deine Region:
     - `us-central1` (USA)
     - `europe-west1` (Europa) - **EMPFOHLEN für Deutschland**
     - `asia-southeast1` (Asien)

5. **Credentials verbinden:**
   - **Credentials:** Klicke auf das Dropdown-Feld
   - **Wähle:** Die Credentials, die du in Schritt 2.2 erstellt hast (`Google Service Account Moritz`)
   - Falls nicht vorhanden: Klicke auf **"Create New"** und folge Schritt 2.2

6. **Temperature (optional):**
   - **Temperature:** `0.3` (für strukturierte Extraktion)
   - Dies macht die AI-Antworten konsistenter

7. **Max Tokens (optional):**
   - **Max Tokens:** `2048` (Standard)

8. **Speichern:** Klicke auf **"Save"** (unten rechts)

✅ **Erwartetes Ergebnis:** Der Chat Model Node ist konfiguriert und mit Credentials verbunden.

📖 **Referenz:** https://docs.n8n.io/integrations/built-in/core-nodes/n8n-nodes-langchain.lmChatGoogleVertex/

---

## 4. AI Agent Node Konfiguration

### Schritt 4.1: AI Agent Node öffnen

1. **Node finden:** Im Workflow findest du den Node **"AI Agent - Vertex AI"** (in der Mitte des Workflows)
2. **Node öffnen:** Klicke auf den Node

### Schritt 4.2: Chat Model verbinden

1. **Settings Tab öffnen:** Klicke auf den **"Settings"** Tab (oben im Panel)

2. **Chat Model verbinden:**
   - **Chat Model *:** (Stern = Pflichtfeld!)
   - Klicke auf **"+"** oder das Dropdown-Feld
   - Wähle **"Google Vertex Chat Model"** (der Node, den du in Schritt 3 konfiguriert hast)
   - Falls nicht sichtbar: Stelle sicher, dass der "Google Vertex Chat Model" Node korrekt konfiguriert ist (Schritt 3)

✅ **Erwartetes Ergebnis:** Der Chat Model ist verbunden.

### Schritt 4.3: Binary Images aktivieren

1. **Parameters Tab öffnen:** Klicke auf den **"Parameters"** Tab

2. **Options Dropdown öffnen:**
   - Scroll nach unten zu **"Options"**
   - Klicke auf das **Dropdown-Menü**, um es zu erweitern

3. **Automatically Passthrough Binary Images aktivieren:**
   - **Finde:** `Automatically Passthrough Binary Images`
   - **Toggle aktivieren:** Klicke auf den Toggle/Switch, um es zu aktivieren (sollte grün/ON sein)
   - ⚠️ **WICHTIG:** Diese Option ermöglicht es dem AI Agent, Bilddaten direkt zu verarbeiten, ohne Base64-Konvertierung!

✅ **Erwartetes Ergebnis:** Die Option ist aktiviert.

### Schritt 4.4: Tools entfernen

1. **Settings Tab öffnen:** Klicke auf den **"Settings"** Tab

2. **Tools Sektion finden:**
   - Scroll zu **"Tools"** Sektion (unterhalb von Chat Model)

3. **Tools prüfen:**
   - Falls Tools vorhanden sind (Liste ist nicht leer):
     - **ENTFERNE ALLE TOOLS** ❌
     - Klicke auf jedes Tool und dann auf **"Remove"** oder **"X"**
   - Falls keine Tools vorhanden (Liste ist leer):
     - ✅ **Perfekt!** Nichts zu tun.

⚠️ **WICHTIG:** Tools können Fehler verursachen (`Cannot read properties of undefined (reading 'includes')`). Deshalb müssen alle Tools entfernt werden!

✅ **Erwartetes Ergebnis:** Die Tools-Liste ist leer.

### Schritt 4.5: Prompt und System Message prüfen

1. **Parameters Tab öffnen:** Klicke auf den **"Parameters"** Tab

2. **Prompt prüfen:**
   - **Text (Prompt):** Sollte einen langen Prompt enthalten, der beschreibt:
     - Extraktion von Kontaktdaten
     - Internet-Recherche und Verifizierung
     - JSON-Format der Antwort
   - Falls leer oder falsch: Die JSON-Datei sollte den korrekten Prompt enthalten

3. **System Message prüfen:**
   - **System Message:** Sollte sein: `Du bist ein Experte für Visitenkarten-Extraktion mit Internet-Recherche-Fähigkeiten...`
   - Falls leer: Füge die System Message aus der JSON-Datei hinzu

4. **Speichern:** Klicke auf **"Save"** (unten rechts)

✅ **Erwartetes Ergebnis:** Prompt und System Message sind korrekt konfiguriert.

📖 **Referenz:** https://docs.n8n.io/integrations/built-in/core-nodes/n8n-nodes-langchain.agent/

---

## 5. Webhook Node Konfiguration

### Schritt 5.1: Business Card Upload Node öffnen

1. **Node finden:** Im Workflow findest du den Node **"Business Card Upload"** (ganz links, der erste Node)
2. **Node öffnen:** Klicke auf den Node

### Schritt 5.2: Response Mode prüfen

1. **Parameters Tab öffnen:** Stelle sicher, dass der **"Parameters"** Tab aktiv ist

2. **Response Mode prüfen:**
   - **Response Mode:** Sollte **"Respond When Last Node Finishes"** oder **"lastNode"** sein
   - Falls anders eingestellt:
     - **Wähle:** "Respond When Last Node Finishes" aus dem Dropdown
     - ⚠️ **WICHTIG:** Diese Einstellung sorgt dafür, dass die Webhook-Response erst gesendet wird, wenn der "Antwort an Website" Node fertig ist!

✅ **Erwartetes Ergebnis:** Response Mode ist auf "Respond When Last Node Finishes" eingestellt.

### Schritt 5.3: Binary Property Name prüfen

1. **Settings Tab öffnen:** Klicke auf den **"Settings"** Tab

2. **Binary Property Name prüfen:**
   - **Binary Property Name:** Sollte `file` sein
   - Falls leer oder anders:
     - **Ändere zu:** `file`

✅ **Erwartetes Ergebnis:** Binary Property Name ist `file`.

### Schritt 5.4: Path prüfen

1. **Parameters Tab öffnen:** Klicke auf den **"Parameters"** Tab

2. **Path prüfen:**
   - **Path:** Sollte `business-card-extraction` sein
   - Falls anders: Ändere zu `business-card-extraction`

3. **Webhook URL notieren:**
   - **Webhook URL:** Wird angezeigt unter dem Path-Feld
   - Sollte sein: `https://n8n2.kortex-system.de/webhook/business-card-extraction`
   - Notiere diese URL - sie wird für die Website-Integration benötigt!

4. **Speichern:** Klicke auf **"Save"** (unten rechts)

✅ **Erwartetes Ergebnis:** Webhook URL ist korrekt und notiert.

📖 **Referenz:** https://docs.n8n.io/integrations/built-in/core-nodes/n8n-nodes-base.webhook/

---

## 6. IF Nodes Verifikation

### Schritt 6.1: "Ist Sample?" Node prüfen

1. **Node öffnen:** Klicke auf den Node **"Ist Sample?"**

2. **Parameters Tab öffnen:** Stelle sicher, dass der **"Parameters"** Tab aktiv ist

3. **Conditions Sektion prüfen:**
   - **Value 1:** Sollte `={{String($json.query.sample)}}` sein (mit Expression-Syntax `={{}}`!)
   - ⚠️ **WICHTIG:** Es muss **NUR EIN "="** sein, NICHT `==`!
   - **Operator:** Sollte **"is not empty"** oder **"notEmpty"** sein
   - **Value 2:** Sollte **LEER** sein

4. **Expression-Toggle prüfen:**
   - **Value 1** sollte das **FX-Symbol** (Expression-Toggle) aktiviert haben (blau)
   - Falls nicht: Klicke auf Value 1 und aktiviere den Expression-Toggle

5. **Falls falsch konfiguriert:**
   - **Value 1:** Aktiviere Expression-Toggle, tippe: `String($json.query.sample)`
   - Sollte zu `={{String($json.query.sample)}}` werden
   - **Operator:** Wähle "is not empty"
   - **Value 2:** LASS LEER!

6. **Speichern:** Klicke auf **"Save"**

✅ **Erwartetes Ergebnis:** "Ist Sample?" Node prüft, ob `sample` Parameter vorhanden ist.

### Schritt 6.2: "Sample 1?" Node prüfen

1. **Node öffnen:** Klicke auf den Node **"Sample 1?"**

2. **Parameters Tab öffnen:** Stelle sicher, dass der **"Parameters"** Tab aktiv ist

3. **Conditions Sektion prüfen:**
   - **Value 1:** Sollte `={{String($json.query.sample)}}` sein (mit Expression-Syntax!)
   - ⚠️ **WICHTIG:** Es muss **NUR EIN "="** sein, NICHT `==`!
   - **Operator:** Sollte **"equals"** oder **"is equal to"** sein
   - **Value 2:** Sollte `1` sein (als String, nicht als Zahl!)

4. **Expression-Toggle prüfen:**
   - **Value 1:** FX-Symbol sollte aktiviert sein (blau)
   - **Value 2:** FX-Symbol sollte **NICHT** aktiviert sein (weiß)

5. **Falls falsch konfiguriert:**
   - **Value 1:** Aktiviere Expression-Toggle, tippe: `String($json.query.sample)`
   - **Operator:** Wähle "equals"
   - **Value 2:** Deaktiviere Expression-Toggle, tippe: `1`

6. **Speichern:** Klicke auf **"Save"**

✅ **Erwartetes Ergebnis:** "Sample 1?" Node prüft, ob `sample === "1"`.

### Schritt 6.3: "Sample 2?" Node prüfen

1. **Node öffnen:** Klicke auf den Node **"Sample 2?"**

2. **Gleiche Konfiguration wie "Sample 1?"** (Schritt 6.2), aber:
   - **Value 2:** Sollte `2` sein (statt `1`)

3. **Speichern:** Klicke auf **"Save"**

✅ **Erwartetes Ergebnis:** "Sample 2?" Node prüft, ob `sample === "2"`.

### Schritt 6.4: "Sample 3?" Node prüfen

1. **Node öffnen:** Klicke auf den Node **"Sample 3?"**

2. **Gleiche Konfiguration wie "Sample 1?"** (Schritt 6.2), aber:
   - **Value 2:** Sollte `3` sein (statt `1`)

3. **Speichern:** Klicke auf **"Save"**

✅ **Erwartetes Ergebnis:** "Sample 3?" Node prüft, ob `sample === "3"`.

📖 **Referenz:** https://docs.n8n.io/integrations/built-in/core-nodes/n8n-nodes-base.if/

---

## 7. Workflow Aktivierung und Test

### Schritt 7.1: Workflow aktivieren

1. **Workflow Editor öffnen:** Stelle sicher, dass du im Workflow-Editor bist

2. **Workflow aktivieren:**
   - **Oben rechts:** Finde den **"Active"** Toggle/Switch
   - **Klicke auf den Toggle**, um den Workflow zu aktivieren (sollte grün/ON sein)

✅ **Erwartetes Ergebnis:** Der Workflow ist aktiv und der Webhook ist erreichbar.

### Schritt 7.2: Webhook URL verifizieren

1. **Webhook URL anzeigen:**
   - Klicke auf den **"Business Card Upload"** Node
   - **Unter dem Path:** Die vollständige Webhook URL wird angezeigt
   - Sollte sein: `https://n8n2.kortex-system.de/webhook/business-card-extraction`

2. **URL kopieren:**
   - Kopiere die vollständige Webhook URL
   - Diese URL wird in der Website verwendet

✅ **Erwartetes Ergebnis:** Webhook URL ist bekannt und kopiert.

### Schritt 7.3: Test-Execution durchführen

1. **Test-Manual-Execution:**
   - **Oben rechts:** Klicke auf **"Execute Workflow"** oder **"Test workflow"**
   - Der Workflow wird manuell ausgeführt

2. **Sample-Parameter testen:**
   - Der Webhook wird mit `sample=1` getestet
   - ⚠️ **ACHTUNG:** Bei manueller Execution werden die IF-Nodes möglicherweise nicht korrekt getestet, da kein Query-Parameter vorhanden ist.

3. **Besser: Über Website testen:**
   - Gehe zu deiner Website: `https://karusocaminar.github.io/kortex-website/kortex-n8n-modal.html`
   - Klicke auf **"Visitenkarte 1"** (Sample 1)
   - Prüfe ob der Workflow läuft

### Schritt 7.4: Execution prüfen

1. **Executions öffnen:**
   - In n8n, klicke auf **"Executions"** im Hauptmenü (oder oben im Workflow-Editor)

2. **Letzte Execution finden:**
   - Die oberste Execution ist die neueste
   - Klicke auf die Execution, um Details zu sehen

3. **Status prüfen:**
   - **Status:** Sollte **"Success"** (grün) sein
   - Falls **"Error"** (rot):
     - Klicke auf den fehlgeschlagenen Node
     - Prüfe die Fehlermeldung
     - Gehe zurück zu den entsprechenden Konfigurationsschritten

4. **Output prüfen:**
   - Klicke auf **"Antwort an Website"** Node
   - **Output:** Sollte JSON mit `type: 'business-card-processed'` und `payload` enthalten

✅ **Erwartetes Ergebnis:** Workflow läuft erfolgreich und gibt korrekte JSON-Responses zurück.

---

## ✅ Abschluss-Checkliste

Verwende diese Checkliste, um sicherzustellen, dass alles korrekt konfiguriert ist:

- [ ] Workflow JSON importiert
- [ ] Google Vertex AI Credentials erstellt und konfiguriert
- [ ] Chat Model Node konfiguriert (Project ID, Model, Location, Credentials)
- [ ] AI Agent Node: Chat Model verbunden
- [ ] AI Agent Node: "Automatically Passthrough Binary Images" aktiviert
- [ ] AI Agent Node: Alle Tools entfernt (Tools-Liste ist leer)
- [ ] AI Agent Node: Prompt und System Message korrekt
- [ ] Webhook Node: Response Mode = "Respond When Last Node Finishes"
- [ ] Webhook Node: Binary Property Name = `file`
- [ ] Webhook Node: Path = `business-card-extraction`
- [ ] "Ist Sample?" Node: Value 1 = `={{String($json.query.sample)}}`, Operator = "notEmpty", Value 2 = leer
- [ ] "Sample 1?" Node: Value 1 = `={{String($json.query.sample)}}`, Operator = "equals", Value 2 = `1`
- [ ] "Sample 2?" Node: Value 1 = `={{String($json.query.sample)}}`, Operator = "equals", Value 2 = `2`
- [ ] "Sample 3?" Node: Value 1 = `={{String($json.query.sample)}}`, Operator = "equals", Value 2 = `3`
- [ ] Workflow aktiviert
- [ ] Webhook URL notiert
- [ ] Test-Execution erfolgreich durchgeführt

---

## 🐛 Häufige Fehler und Lösungen

### Fehler 1: "Cannot read properties of undefined (reading 'includes')"

**Ursache:** Tools sind im AI Agent Node vorhanden.

**Lösung:**
1. Öffne "AI Agent - Vertex AI" Node
2. Gehe zu "Settings" Tab
3. Entferne alle Tools (Schritt 4.4)

---

### Fehler 2: IF Nodes landen immer im False Branch

**Ursache:** `leftValue` hat `==` statt `=`.

**Lösung:**
1. Öffne die IF Node (z.B. "Sample 1?")
2. Prüfe Value 1: Sollte `={{String($json.query.sample)}}` sein (nur ein "="!)
3. Falls `==`: Ändere zu `={{String($json.query.sample)}}` (Schritt 6.2)

---

### Fehler 3: "500 Internal Server Error" oder "Binary-Daten fehlen"

**Ursache:** Binary-Daten kommen nicht beim AI Agent an.

**Lösung:**
1. Prüfe "Setze Sample-Info" Node: Code sollte korrekt sein
2. Prüfe IF Nodes: Alle sollten korrekt konfiguriert sein (Schritt 6)
3. Prüfe "Lade Sample X" Nodes: Response Format sollte "file" sein

---

### Fehler 4: Workflow gibt keine Response zurück

**Ursache:** Webhook Response Mode ist falsch eingestellt.

**Lösung:**
1. Öffne "Business Card Upload" Node
2. Prüfe Response Mode: Sollte "Respond When Last Node Finishes" sein (Schritt 5.2)

---

## 📚 Weitere Ressourcen

- **n8n IF Node Dokumentation:** https://docs.n8n.io/integrations/built-in/core-nodes/n8n-nodes-base.if/
- **n8n Webhook Dokumentation:** https://docs.n8n.io/integrations/built-in/core-nodes/n8n-nodes-base.webhook/
- **n8n AI Agent Dokumentation:** https://docs.n8n.io/integrations/built-in/core-nodes/n8n-nodes-langchain.agent/
- **n8n Credentials:** https://docs.n8n.io/credentials/
- **n8n Expressions:** https://docs.n8n.io/code/expressions/

---

## 🎉 Erfolg!

Wenn alle Schritte abgeschlossen sind und die Checkliste alle Punkte erfüllt, sollte dein Workflow jetzt vollständig funktionieren!

Bei Problemen: Prüfe die "Executions" in n8n, um zu sehen, welcher Node fehlschlägt, und gehe zurück zu den entsprechenden Konfigurationsschritten.

