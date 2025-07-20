# 📰 Multi-API Newsletter Generator

Ein intelligentes Newsletter-System für deutsche Städte mit Multi-API Integration und automatischer Fact-Extraction.

## 🚀 Übersicht

Das Newsletter-System kombiniert mehrere KI-APIs zur automatischen Generierung lokaler Newsletter. Es sammelt aktuelle Informationen aus verschiedenen Quellen, extrahiert strukturierte Facts und generiert professionelle Newsletter in drei verschiedenen Stilen.

### ✨ Hauptfeatures

- **Multi-API Integration**: Firecrawl (Scraping) + Claude (Web Search) + Perplexity (Cross-Validation) + Gemini (Generation)
- **3 Newsletter-Levels**: Compact (300 Wörter) | Standard (500 Wörter) | Detailed (800+ Wörter)
- **Fact-Extraction**: Automatische Extraktion konkreter Daten (Temperaturen, Termine, Preise)
- **Gradio Web-UI**: Benutzerfreundliche Browser-Oberfläche
- **Audit-Trail**: Vollständige Nachvollziehbarkeit aller API-Calls
- **Google Drive Integration**: Strukturierte Speicherung aller Daten

## 🏗️ Systemarchitektur

### Foundation-System
```
ConfigManager          → YAML-basierte Konfiguration (Städte, Kategorien, APIs)
TimeContextManager     → Intelligente Datums-/Zeit-Kontextualisierung
DataPersistenceManager → Strukturierte Google Drive Speicherung
```

### Worker V2 Suite
```
Firecrawl Worker V2    → Website-Scraping + Search
Claude Worker V2       → Web Search mit Citations
Perplexity Worker V2   → Multi-Angle Search + Cross-Validation  
Gemini Worker V2       → Newsletter-Generierung + Content-Synthese
```

### Content Processing
```
Enhanced Content Processor → Komplexe Regex-basierte Fact-Extraction (deprecated)
Simple Content Processor   → Einfache, wartbare Fact-Extraction (empfohlen)
```

### Main Controller
```
5-Phasen Pipeline:
1. Planning      → Config-basierte Kategorien + Worker-Strategien
2. Data Collection → Multi-API orchestrierte Calls  
3. Synthesis     → Content Quality Assessment + Diversity Score
4. Generation    → Newsletter-Erstellung aus allen Quellen
5. Finalization  → Session-Archivierung + Audit-Trail
```

## 📦 Installation & Setup

### 1. Requirements Installation
```python
# In Google Colab ausführen:
import subprocess
import sys

requirements = [
    "firecrawl-py",
    "google-generativeai", 
    "anthropic",
    "requests",
    "pandas",
    "asyncio",
    "nest-asyncio",
    "ipywidgets",
    "python-dotenv",
    "gradio"
]

for package in requirements:
    subprocess.check_call([sys.executable, "-m", "pip", "install", package])
```

### 2. API Keys Setup
Erstelle folgende Secrets in Google Colab:
```
FIRECRAWL_API     → Firecrawl API Key
ANTHROPIC_API     → Claude API Key  
PERPLEXITY_API_KEY → Perplexity API Key
GOOGLE_API_KEY    → Gemini API Key
```

### 3. Google Drive Setup
```python
# Automatisch beim ersten Start - erstellt:
/content/drive/MyDrive/Newsletter_System/
├── configs/           # YAML-Konfigurationsdateien
├── data/sessions/     # Session-basierte Datensammlung  
├── templates/         # Newsletter-Templates
└── logs/             # System-Logs
```

## 🎯 Quick Start

### Option 1: Vollständige Pipeline
```python
# 1. Alle Zellen ausführen (0a bis 6)
# 2. Main Controller verwenden:
demo_result = main_controller.generate_complete_newsletter(
    location="münchen",
    categories=["wetter", "nachrichten", "events", "sport"]
)
```

### Option 2: Gradio Web-UI  
```python
# 1. Alle Zellen ausführen
# 2. UI starten:
free_port = find_free_port()
launch_newsletter_ui(port=free_port)
```

### Option 3: Einzelne Worker
```python
# Simple Gemini Worker direkt verwenden:
newsletter = simple_gemini_worker.generate_simple_newsletter(
    location="münchen",
    newsletter_style="standard"
)
```

## 📚 Zellen-Übersicht

### Foundation-System (Zellen 0a-0e)
- **Zelle 0a**: Requirements Installation
- **Zelle 0b**: Google Drive Setup + Ordnerstruktur
- **Zelle 0c**: ConfigManager Implementation  
- **Zelle 0d**: TimeContextManager Implementation
- **Zelle 0e**: DataPersistenceManager Implementation

### API Configuration (Zelle 1)
- **Zelle 1**: API Keys Setup aus Google Colab Secrets

### Worker V2 Suite (Zellen 2-5b)
- **Zelle 2**: Firecrawl Worker V2 - Website-Scraping + Search
- **Zelle 3**: Claude Worker V2 - Web Search mit Citations
- **Zelle 4**: Perplexity Worker V2 - Multi-Angle Search
- **Zelle 5a**: Enhanced/Simple Content Processor (2 Varianten)
- **Zelle 5b**: Simple Gemini Worker - Newsletter-Generierung

### Orchestrierung & UI (Zellen 6-7)
- **Zelle 6**: Main Controller - 5-Phasen Pipeline
- **Zelle 7**: Gradio Web-UI - Browser-Interface

## 🎨 Newsletter-Stile

### Compact (250-350 Wörter)
- Fokus auf wichtigste Informationen
- 1-2 Sätze pro Kategorie
- Schnell lesbar
- Keine Quellenangaben

### Standard (400-600 Wörter)  
- Ausgewogene Berichterstattung
- 2-4 Sätze pro Kategorie
- Konkrete Facts integriert
- Professioneller Ton

### Detailed (600-900 Wörter)
- Umfassende, faktenbasierte Berichterstattung
- 3-6 Sätze pro Kategorie  
- Alle verfügbaren Details
- Quellenangaben in eckigen Klammern

## 📊 Unterstützte Kategorien

### High Priority
- **Wetter**: Aktuelle Vorhersage, Temperaturen, Warnungen
- **Nachrichten**: Lokale News, Politik, Wirtschaft  
- **Verkehr**: Störungen, ÖPNV, Baustellen
- **Events**: Veranstaltungen, Konzerte, Festivals

### Medium Priority  
- **Sport**: Lokale Vereine, Ergebnisse, Spieltermine
- **Rathaus**: Offizielle Mitteilungen, Bekanntmachungen
- **Schulen**: Bildungsnachrichten, Mitteilungen

## 🏙️ Unterstützte Städte

Aktuell konfiguriert für:
- **München** (Vollständig getestet)
- **Berlin** (Konfiguriert)
- **Hamburg** (Konfiguriert) 
- **Köln** (Konfiguriert)
- **Frankfurt** (Konfiguriert)

### Neue Städte hinzufügen
```yaml
# In configs/locations.yaml:
neue_stadt:
  official_names: ["Neue Stadt", "New City"]
  url_slug: "neue-stadt"
  official_domains: ["neue-stadt.de"]
  timezone: "Europe/Berlin"
  region_context: "Deutschland"
  population: 100000
  type: "stadt"
```

## 🔧 Konfiguration

### API-spezifische Parameter
```yaml
# configs/api_settings.yaml
claude:
  model: "claude-3-5-haiku-latest"
  max_tokens: 2000
  web_search:
    max_uses: 5

perplexity:
  model: "sonar-pro"
  temperature: 0.2
  
gemini:
  model: "gemini-2.0-flash-exp"
  temperature: 0.3
  max_output_tokens: 2000
```

### Kategorie-Templates
```yaml
# configs/categories.yaml
wetter:
  priority: "high"
  time_sensitivity: "today" 
  search_templates:
    claude_web: "aktuelles Wetter {location} heute {date_context}"
    perplexity: "Wetter Vorhersage {location} {date_context}"
  fallback_urls:
    - "https://www.wetter.com/deutschland/{location_slug}"
```

## 📈 Performance & Monitoring

### Typische Performance
- **Single Newsletter**: 15-30 Sekunden
- **Style-Vergleich (3x)**: 45-90 Sekunden  
- **Complete Pipeline**: 60-120 Sekunden

### Monitoring Features
- Detaillierte Progress-Updates im UI
- Console-Logging aller API-Calls
- Performance-Timing für jeden Schritt
- Error-Recovery mit spezifischen Fehlermeldungen

## 🐛 Troubleshooting

### Häufige Probleme

#### 1. Gemini API hängt
```python
# Debug-Test:
response = simple_gemini_worker.model.generate_content("Test")
```
**Lösungen:**
- API-Key prüfen: `print(api_config.gemini_key[:10])`
- Alternatives Model: `gemini-1.5-flash`
- Rate-Limit warten: 1-2 Minuten Pause

#### 2. Firecrawl Search Fehler  
```
ERROR: nothing to repeat at position 0
```
**Lösung:** Regex-Pattern in Enhanced Content Processor → Simple Content Processor verwenden

#### 3. Progress Bar hängt
- **DEBUG-Button** im UI verwenden
- Console-Output prüfen für genaue Fehlerstelle
- Worker einzeln testen

#### 4. Google Drive Permissions
```python
# Drive neu mounten:
from google.colab import drive
drive.mount('/content/drive', force_remount=True)
```

### Error Recovery
- **Automatic Fallbacks**: Simple Content Processor falls Enhanced fehlt
- **Graceful Degradation**: System läuft auch mit einzelnen API-Ausfällen
- **Retry Logic**: Automatische Wiederholung bei temporären Fehlern

## 📁 Datenstruktur

### Session-basierte Speicherung
```
/data/sessions/YYYY-MM-DD_HH-MM_location/
├── raw_responses/         # Original API-Responses
├── processed_content/     # Verarbeitete Inhalte pro Kategorie
├── queries/              # Query-Historie mit IDs
├── metadata/             # Content-Metadaten  
├── final_newsletter/     # Generierte Newsletter
├── logs/                 # Session-Logs
└── session_meta.json     # Session-Zusammenfassung
```

### Audit-Trail Features
- **Query-IDs**: Jeder API-Call eindeutig identifizierbar
- **Raw Response Storage**: Original-Antworten archiviert
- **Content Lineage**: Nachverfolgung von Source → Facts → Newsletter
- **Performance Metrics**: Timing aller Pipeline-Schritte

## 🔮 Roadmap

### v2.0 Features (geplant)
- [ ] **Automatisierung**: Scheduler für tägliche Newsletter
- [ ] **Email-Distribution**: Automatischer Versand
- [ ] **Social Media Integration**: Twitter/LinkedIn Posting
- [ ] **Multi-Language Support**: Englische Newsletter
- [ ] **Analytics Dashboard**: Newsletter-Performance Tracking

### v2.1 Features
- [ ] **RSS-Feed Integration**: Weitere Content-Quellen
- [ ] **Image-Generation**: KI-generierte Newsletter-Bilder  
- [ ] **PDF-Export**: Professional Newsletter PDFs
- [ ] **API-Endpoint**: REST-API für externe Integration

## 🤝 Contributing

### Entwicklung
1. **Neue Worker**: Implementiere Worker-Interface
2. **Neue Kategorien**: Erweitere `configs/categories.yaml`
3. **Neue Städte**: Füge zu `configs/locations.yaml` hinzu
4. **UI-Verbesserungen**: Erweitere Gradio-Interface

### Testing
- **Worker-Tests**: Jede Zelle hat integrierte Tests
- **End-to-End Tests**: Zelle 7 Demo-Pipeline
- **Performance-Tests**: Console-Timing Ausgaben

## 📄 Lizenz

Dieses Projekt ist für Bildungs- und Forschungszwecke entwickelt.

**API-Usage Notice:** 
- Respektiere API-Limits der verwendeten Services
- Firecrawl: Respektiere robots.txt
- Claude/Perplexity/Gemini: Commercial-Usage gemäß API-Terms

## 🆘 Support

### Bei Problemen:
1. **DEBUG-Button** im UI verwenden
2. **Console-Output** prüfen  
3. **Worker einzeln testen** (siehe Troubleshooting)
4. **Issue erstellen** mit Error-Details

### Hilfreiche Debug-Commands:
```python
# System-Status prüfen:
main_controller.get_system_status()

# Content-Processing testen:
simple_content_processor.get_processing_summary()

# Worker-Health-Check:
simple_gemini_worker.get_simple_newsletter_summary()
```

---

**Entwickelt mit ❤️ für automatisierte, lokale Newsletter-Generierung**

*Last Updated: Juli 2025 | Version: 1.0*
