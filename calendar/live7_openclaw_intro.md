# Warsztat #7 - OpenClaw: Wstęp do prezentacji

## HOOK NA START (2-3 minuty)

### Slajd 1: Otwarcie

**[Ekran na czarno, potem pojawia się tekst]**

```
60 000 gwiazdek na GitHubie.
W 72 godziny.
```

**Mów:**
> "Styczeń 2026. Austriacki developer Peter Steinberger wypuszcza projekt open source. W trzy dni - 60 tysięcy gwiazdek. Andrej Karpathy - jeden z najbardziej wpływowych ludzi w AI - nazywa to 'najbardziej niesamowitą rzeczą zbliżoną do sci-fi'."

---

### Slajd 2: Historia nazwy

**[Timeline graficzny]**

```
Clawdbot → Moltbot → OpenClaw
```

**Mów:**
> "Projekt oryginalnie nazywał się Clawdbot - nawiązanie do Claude'a. Anthropic złożyło skargę o znak towarowy. Zmiana na Moltbot. Potem na OpenClaw. I właśnie wtedy eksplodował."

---

### Slajd 3: Czym jest OpenClaw?

**[Wizualizacja: telefon + komputer + różne ikony aplikacji]**

**Mów:**
> "OpenClaw to nie jest narzędzie dla programistów. To osobisty system operacyjny AI. Daemon działający na Twoim komputerze, który łączy się z WhatsApp, Telegram, Slack, Discord, Signal, iMessage - ponad 30 platform."

**Bullet points na ekranie:**
- Zarządzanie kalendarzem
- Podsumowywanie maili
- Kontrola smart home
- Automatyzacja powtarzalnych workflow

---

### Slajd 4: Model-agnostic

**[Loga modeli: Claude, GPT-4o, DeepSeek, Gemini, Ollama]**

**Mów:**
> "Najlepsza część? OpenClaw jest model-agnostic. Podpinasz Claude'a, GPT-4o, DeepSeeka, Gemini - albo lokalne modele przez Ollama. Płacisz tylko za tokeny API które zużyjesz. Żadnej subskrypcji."

---

### Slajd 5: Persistent Memory

**[Porównanie: Claude Code vs OpenClaw]**

```
Claude Code: Reset pamięci między sesjami
OpenClaw: Pamięta przez tygodnie (lokalnie)
```

**Mów:**
> "Claude Code resetuje pamięć między sesjami. OpenClaw pamięta. Przez tygodnie. Cała pamięć lokalnie na Twoim komputerze."

---

### Slajd 6: ClawHub - Ekosystem

**[Liczba: 5,700+ skills]**

**Mów:**
> "ClawHub - registry z ponad 5700 community-built skills. Od kontroli Spotify, przez zarządzanie listą zakupów, po uruchamianie komend shell. Społeczność buduje narzędzia, które możesz od razu używać."

---

## SEKCJA: BEZPIECZEŃSTWO (5-7 minut)

### Slajd 7: Słoń w pokoju

**[Czerwony alert / warning symbol]**

```
⚠️ BEZPIECZEŃSTWO
```

**Mów:**
> "OK, teraz słoń w pokoju. Cisco, BitSight i inne firmy security nazywają OpenClaw 'security nightmare dla casual users'. I mają rację. Prompt injection, skompromitowane skills - realne zagrożenia."

---

### Slajd 8: CVE-2026-25253

**[Czerwony ekran z numerem CVE]**

```
CVE-2026-25253
CVSS: 8.8 (HIGH)
Remote Code Execution
```

**Mów:**
> "Na początku 2026 roku odkryto krytyczną podatność - Remote Code Execution. CVSS 8.8. Dlatego mówimy o tym na samym początku. OpenClaw to potężne narzędzie, ale wymaga odpowiedzialnego użytkowania."

---

### Slajd 9: Rozwiązanie - Izolacja

**[Docker logo + diagram izolacji]**

**Mów:**
> "Rozwiązanie? Izolacja. Docker albo maszyna wirtualna. Nie instalujesz OpenClaw bezpośrednio na swoim systemie. Uruchamiasz w kontenerze z ograniczonym dostępem."

**Bullet points:**
- Docker sandbox = ograniczony blast radius
- Kontener nie ma dostępu do całego systemu
- Nawet jeśli skill jest złośliwy - izolacja go zatrzymuje

---

### Slajd 10: Best Practices

**[Lista z checkmarkami]**

**Mów:**
> "Kilka żelaznych zasad bezpieczeństwa:"

```
✅ Docker z non-root user (node, uid 1000)
✅ Publikuj port tylko na localhost: 127.0.0.1:1618:1618
✅ NIGDY nie wystawiaj gateway na publiczny internet
✅ Mountuj tylko niezbędne katalogi
✅ Klucze API przez env variables, nie hardcode
✅ Dla remote access: Tailscale/VPN, nie otwarty port
```

---

## SEKCJA: INSTALACJA KROK PO KROKU (10-15 minut)

### Slajd 11: Instalacja - Przegląd

**[3 etapy wizualnie]**

```
1. Docker Setup
2. Konfiguracja
3. Podłączenie modelu AI
```

**Mów:**
> "Instalacja w trzech krokach. Docker setup, konfiguracja, podłączenie modelu. Zróbmy to razem."

---

### Slajd 12: Krok 1 - Docker

**[Terminal / kod]**

```bash
# Klonowanie repo
git clone https://github.com/openclaw/openclaw.git
cd openclaw

# Uruchomienie setup script
./docker-setup.sh
```

**Mów:**
> "Klonujemy repo, uruchamiamy docker-setup.sh. Skrypt używa Docker Compose i tworzy dwa foldery jako volumes."

---

### Slajd 13: Struktura folderów

**[Diagram folderów]**

```
~/.openclaw/           # Konfiguracja
├── config/            # Ustawienia
├── memory/            # Pamięć agenta
└── api-keys/          # Klucze (bezpiecznie!)

~/openclaw/workspace/  # Workspace
└── [pliki tworzone przez agenta]
```

**Mów:**
> "Dwa główne foldery. ~/.openclaw dla konfiguracji, pamięci i kluczy API. ~/openclaw/workspace jako miejsce pracy agenta - tu zapisuje pliki które tworzy."

---

### Slajd 14: Pre-built Image (alternatywa)

**[Terminal]**

```bash
# Zamiast budować lokalnie, możesz pobrać gotowy obraz
export OPENCLAW_IMAGE=openclaw/openclaw:latest
./docker-setup.sh
```

**Mów:**
> "Jeśli nie chcesz budować lokalnie - ustaw OPENCLAW_IMAGE przed uruchomieniem skryptu. Pobierze gotowy obraz zamiast kompilować."

---

### Slajd 15: Krok 2 - Konfiguracja sieci

**[docker-compose.yml fragment]**

```yaml
services:
  openclaw:
    ports:
      - "127.0.0.1:1618:1618"  # TYLKO localhost!
    # NIE: - "1618:1618"  # To wystawia na świat!
```

**Mów:**
> "Krytyczne: port bindujemy TYLKO do localhost. 127.0.0.1:1618:1618. Nie wystawiamy na świat. Jeśli potrzebujesz remote access - Tailscale, SSH tunnel, VPN."

---

### Slajd 16: Krok 3 - Klucze API

**[Przykład .env]**

```bash
# ~/.openclaw/.env
ANTHROPIC_API_KEY=sk-ant-xxxxx
OPENAI_API_KEY=sk-xxxxx

# Docker sandbox proxy automatycznie wstrzykuje klucze
# Nigdy nie są eksponowane wewnątrz sandboxa
```

**Mów:**
> "Klucze API w pliku .env lub zmiennych środowiskowych. Docker sandbox proxy automatycznie je wstrzykuje - klucze nigdy nie są widoczne wewnątrz kontenera."

---

### Slajd 17: Reverse Proxy (dla zaawansowanych)

**[Diagram: Internet → Nginx → Docker → OpenClaw]**

**Mów:**
> "Dla bardziej zaawansowanych setup'ów: Nginx jako reverse proxy z SSL od Let's Encrypt. Ale to opcjonalne - na start wystarczy lokalny dostęp."

---

### Slajd 18: Weryfikacja instalacji

**[Terminal output]**

```bash
# Sprawdź czy kontener działa
docker ps

# Sprawdź logi
docker logs openclaw

# Test połączenia
curl http://localhost:1618/health
```

**Mów:**
> "Weryfikacja: docker ps pokaże działający kontener, logi potwierdzą start, curl na /health zwróci status. Jesteśmy gotowi."

---

## PODSUMOWANIE INTRO

### Slajd 19: Co będziemy robić dalej

**[Agenda reszty warsztatu]**

```
→ Podłączenie pierwszej platformy (Telegram/Discord)
→ Pierwszy skill od zera
→ Integracja z Claude/GPT
→ Automatyzacja prawdziwego workflow
→ Q&A
```

**Mów:**
> "OK, mamy zainstalowane OpenClaw w bezpiecznym środowisku Docker. Teraz przechodzimy do konkretów - podłączymy pierwszą platformę i zbudujemy pierwszy skill na żywo."

---

## ŹRÓDŁA

- [DataCamp: OpenClaw vs Claude Code](https://www.datacamp.com/blog/openclaw-vs-claude-code)
- [Docker: Run OpenClaw Securely in Docker Sandboxes](https://www.docker.com/blog/run-openclaw-securely-in-docker-sandboxes/)
- [AIML API: Running OpenClaw in Docker](https://aimlapi.com/blog/running-openclaw-in-docker-secure-local-setup-and-practical-workflow-guide)
- [OpenClaw Docker Docs](https://docs.openclaw.ai/install/docker)
- [Simon Willison: Running OpenClaw in Docker](https://til.simonwillison.net/llms/openclaw-docker)

---

## NOTATKI DLA PROWADZĄCEGO

**Timing:**
- Hook/Intro: 5 min
- Bezpieczeństwo: 7 min
- Instalacja: 15 min
- **Total intro: ~25-30 min**

**Przygotuj przed live:**
- [ ] Docker zainstalowany i działający
- [ ] Konto z kluczem API (Anthropic lub OpenAI)
- [ ] Testowy kontener OpenClaw gotowy do pokazania
- [ ] Backup screenshoty gdyby coś nie działało

**Key messages:**
1. OpenClaw to "Swiss Army knife" AI - ogólnego przeznaczenia, nie tylko coding
2. ZAWSZE w Docker/izolacji - bezpieczeństwo to priorytet
3. Model-agnostic = elastyczność i kontrola kosztów
4. Społeczność 5700+ skills = nie musisz wszystkiego pisać sam
