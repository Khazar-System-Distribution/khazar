<p align="center">
  <img src="distro/branding/khazar-logo.png" width="160" alt="KhazarOS Logo">
</p>

<h1 align="center">KhazarOS</h1>
<p align="center"><strong>AI-Powered Linux Desktop</strong><br>
<sub>Fedora Silverblue · GNOME · C11 · systemd</sub></p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-GPLv3-blue" alt="GPLv3"></a>
  <img src="https://img.shields.io/badge/language-C11-%23e94560" alt="C11">
  <img src="https://img.shields.io/badge/build-0_errors-success" alt="0 errors">
  <img src="https://img.shields.io/badge/tests-94%2B-brightgreen" alt="94+ tests">
  <img src="https://img.shields.io/badge/platform-Linux%20x86__64-orange" alt="x86_64">
  <img src="https://img.shields.io/badge/Silverblue-immutable-blueviolet" alt="Silverblue">
</p>

<hr>

<h3 align="center">"firefox aç" · "wifi söndür" · "səs artır" · "sistemi yenilə"</h3>
<p align="center">Kompüteri səslə və ya mətnlə idarə edin.<br>
Azərbaycan türkcəsi + İngilis dəstəyi. Tam açıq qaynaq. Offline işləyir.</p>

---

## Niyə KhazarOS?

| Problem | KhazarOS Həlli |
|---------|---------------|
| Terminal istifadəsi çətindir | Təbii dildə yaz: `kha "firefox aç"` |
| Kompüter konfiqurasiyası qarışıqdır | AI sənin yerinə idarə edir |
| Səsli köməkçilər internet tələb edir | Tamamilə offline, lokal AI model |
| Mövcud AI-lər qapalı qaynaqdır | GPLv3 — hər sətir kod açıqdır |
| Mövcud AI-lər yalnız İngiliscədir | Azərbaycan türkcəsi + İngilis |
| Təhlükəsizlik sonradan düşünülür | Policy Engine — hər əmr icazədən keçir |

**KhazarOS digər həllərdən nə ilə fərqlənir:**

| Xüsusiyyət | KhazarOS | Windows Copilot | macOS Siri | Ubuntu |
|-----------|----------|----------------|------------|--------|
| Açıq qaynaq | ✅ GPLv3 | ❌ | ❌ | ✅ |
| Offline AI | ✅ Local GGUF | ❌ Cloud | ❌ Cloud | — |
| Türkcə dəstək | ✅ Azərbaycan | ⚠️ Qismən | ⚠️ | — |
| Təhlükəsizlik qatı | ✅ Policy Engine | ❌ | ❌ | ❌ |
| İmmutable OS | ✅ rpm-ostree | ❌ | ❌ | ❌ |
| İstifadəçi interfeysi | GNOME Shell | Windows | macOS | GNOME |
| Səsli əmr | ✅ (gələcək) | ✅ | ✅ | ❌ |

---

## Demo

```bash
$ kha "firefox aç"
  Status: success
  Result: opened firefox

$ kha "wifi söndür"
  Status: success
  Result: WiFi interface disabled

$ kha "səs artır"
  Status: success
  Result: volume +5%

$ kha "sistemi yenilə"
  Status: success
  Result: apt update && apt upgrade

$ kha "shutdown"
  Status: error
  Error: POLICY_DENIED — system shutdown requires policy check

$ kha status
  orchestrator       ● active
  rule-engine        ● active
  policy-engine      ● active
  intent-classifier  ○ inactive (no LLM model)
  desktop-agent      ● active
  package-agent      ● active
  network-agent      ● active
  power-agent        ● active
  audio-agent        ● active
```

---

## Arxitektura

```
┌─────────────────────────────────────────────────────────────────┐
│                        KhazarOS Desktop                          │
│                                                                  │
│  ┌────────────┐          ┌──────────────────────────────────┐   │
│  │   GNOME    │          │      Khazar Daemon (systemd)      │   │
│  │   Shell    │          │                                   │   │
│  │            │          │  ┌─────────┐   ┌──────────────┐  │   │
│  │  Panel AI  │          │  │  Tier 0 │   │   Tier 1     │  │   │
│  │  Icon  ────┼──────────┼─►│  Rule   │   │  Classifier  │  │   │
│  │            │          │  │  Engine │──►│  (LLM)       │  │   │
│  │  Ctrl+     │          │  │ 49 test │   │  mock+llama  │  │   │
│  │  Space ────┼──────────┼─►└────┬────┘   └──────┬───────┘  │   │
│  │            │          │       │                 │          │   │
│  └────────────┘          │  ┌────▼─────────────────▼──────┐  │   │
│                          │  │     Orchestrator v0.3       │  │   │
│                          │  │  Intent routing + Registry  │  │   │
│                          │  └────┬──────────────┬─────────┘  │   │
│                          │       │              │             │   │
│                          │  ┌────▼────┐   ┌─────▼────────┐  │   │
│                          │  │ Policy  │   │   5 Agent    │  │   │
│                          │  │ Engine  │   │  Desktop     │  │   │
│                          │  │ 13 rule │   │  Package     │  │   │
│                          │  │ ALLOW/  │   │  Network     │  │   │
│                          │  │ DENY    │   │  Power       │  │   │
│                          │  └─────────┘   │  Audio       │  │   │
│                          │                 └──────────────┘  │   │
│                          │  ┌──────────────────────────────┐ │   │
│                          │  │     Model Runtime            │ │   │
│                          │  │  GGUF + llama.cpp inference   │ │   │
│                          │  └──────────────────────────────┘ │   │
│                          └───────────────────────────────────┘   │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────────┐ │
│  │  Baza:  Fedora Silverblue 40 (rpm-ostree, atomic, immutable) │ │
│  │  Build: podman → bootc → ISO                                 │ │
│  └─────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

## Tam Pipeline (5 addım)

```
Addım 1: İstifadəçi sorğusu
    $ kha "firefox aç"
    → JSON → Unix Socket → Orchestrator

Addım 2: Intent təyini (Tier 0)
    Rule Engine: 31 intent, 5 mərhələ (cache→regex→token→intent→alias)
    "firefox aç" → open_application, target=firefox
    UNKNOWN → Tier 1 (LLM Classifier)

Addım 3: Agent tapma (Registry)
    capability: open_application → agent: desktop-agent
    socket: /run/khazar/desktop-agent.sock

Addım 4: Təhlükəsizlik yoxlaması (Policy Engine)
    agent=desktop-agent, cap=open_application
    → qayda: [rule.allow_desktop] priority=10 → ALLOW
    → cavab: "application control allowed"

Addım 5: İcra (Agent Dispatch)
    Orchestrator → desktop-agent socket → fork+exec firefox
    → Cavab: {"status":"success","message":"opened firefox"}
```

## Dəstəklənən Əmrlər (31 intent)

### Tətbiq İdarəsi
| Əmr nümunəsi | Intent | Agent |
|-------------|--------|-------|
| `firefox aç` | open_application | desktop-agent |
| `telegram bağla` | close_application | desktop-agent |
| `pəncərə böyüt` | window_maximize | desktop-agent |

### Paket İdarəsi
| Əmr nümunəsi | Intent | Agent |
|-------------|--------|-------|
| `steam quraşdır` | install_package | package-agent |
| `vlc sil` | remove_package | package-agent |
| `firefox ara` | search_package | package-agent |
| `sistemi yenilə` | system_update | package-agent |

### Şəbəkə
| Əmr nümunəsi | Intent | Agent |
|-------------|--------|-------|
| `wifi aç` | network_enable | network-agent |
| `wifi bağla` | network_disable | network-agent |
| `şəbəkə durumu` | network_status | network-agent |

### Enerji
| Əmr nümunəsi | Intent | Agent |
|-------------|--------|-------|
| `söndür` | system_shutdown | power-agent |
| `yenidən başlat` | system_reboot | power-agent |
| `yuxu` | system_suspend | power-agent |
| `kilidlə` | system_lock | power-agent |
| `çıxış` | system_logout | power-agent |

### Audio
| Əmr nümunəsi | Intent | Agent |
|-------------|--------|-------|
| `səs artır` | volume_up | audio-agent |
| `səs azalt` | volume_down | audio-agent |
| `səssiz` | volume_mute | audio-agent |

### Ekran + Fayl + Digər
| Əmr nümunəsi | Intent | Agent |
|-------------|--------|-------|
| `ekran görüntüsü` | screenshot | — |
| `sənədləri aç` | file_open | — |
| `fayl tap` | file_search | — |
| `parlaqlıq artır` | brightness_up | — |

> **Cəmi 31 intent** — hamısı Tier 0 ilə tanınır. Bütün mərhələlər 1ms-dən az müddətdə işləyir.

---

## Quraşdırma

### Yol A: RPM Paketi (Fedora istifadəçiləri)

```bash
# RPM qur
rpmbuild -ba distro/rpm/khazar.spec

# rpm-ostree ilə quraşdır (Silverblue)
rpm-ostree install khazar-0.1.0-1.fc40.x86_64.rpm
systemctl reboot
systemctl enable khazar.target --now
```

### Yol B: KhazarOS ISO (Sıfırdan)

```bash
git clone https://github.com/Khazar-System-Distribution/khazar
cd khazar

# ISO yarat
make -C distro build-iso
# → distro/iso/output/khazaros-0.1.0.iso

# USB-yə yaz
make -C distro flash DISK=/dev/sdX

# QEMU-da test
make -C distro test-qemu
```

### Yol C: Manual (Mövcud Linux-a)

```bash
git clone https://github.com/Khazar-System-Distribution/khazar
cd khazar
make all                                    # 0 error, 94+ test
sudo bash distro/install.sh                 # Hamısını quraşdırır
systemctl enable khazar.target --now        # Daemon-u başladır
kha "firefox aç"                            # İlk əmr!
```

---

## Developer Sənədləri

### Repo Strukturu

```
khazar/                                 # Monorepo — 167 fayl, 17K+ sətir
├── sdk/                                # Agent SDK (IPC, logger, events)
│   ├── include/common.h                # Ortaq tip + sabit
│   ├── ipc/                            # Unix socket client+server (epoll)
│   ├── agent/                          # Agent lifecycle (register, heartbeat)
│   └── events/                         # In-process pub/sub
├── components/                         # Server komponentlər
│   ├── orchestrator/                   # Tier 0+1 router (v0.3)
│   │   ├── registry/                   # Agent qeydiyyatı + capability lookup
│   │   ├── rule_client/                # Rule Engine IPC client
│   │   ├── policy_client/              # Policy Engine IPC client
│   │   ├── agent_client/               # Agent dispatch (socket connect+send)
│   │   └── intent_client/              # Tier 1 classifier client
│   ├── rule-engine/                    # Deterministik intent təyini (49 test)
│   │   ├── cache/                      # LRU keş (O(1), 256 əmr)
│   │   ├── matcher/                    # POSIX regex matching (1024 rule)
│   │   ├── tokens/                     # Açar söz lookup + scoring
│   │   ├── intent/                     # Tam əmr → intent cədvəli (2048 entry)
│   │   ├── alias/                      # Alternativ ad həlli (1024 alias)
│   │   └── fuzzy/                      # Levenshtein təxmini uyğunluq
│   ├── policy-engine/                  # Təhlükəsizlik siyasəti (13 qayda)
│   │   └── policy/                     # fnmatch agent+capability matching
│   ├── model-runtime/                  # LLM inference server
│   │   └── inference/                  # Mock + llama.cpp stub
│   └── intent-classifier/              # Tier 1 LLM fallback
├── agents/                             # 5 agent
│   ├── desktop-agent/                  # fork+exec tətbiq açma
│   ├── package-agent/                  # apt/pacman paket idarəsi
│   ├── network-agent/                  # nmcli WiFi/ethernet
│   ├── power-agent/                    # systemctl/loginctl enerji
│   └── audio-agent/                    # pactl səs idarəsi
├── protocol/                           # IPC kontraktları (JSON Schema)
├── distro/                             # Distro qurma alətləri
│   ├── bootc/Containerfile             # OS image definition
│   ├── gnome/                          # GNOME özəlləşdirmə
│   │   ├── theme/Khazar-dark/          # Shell + GTK4 CSS
│   │   ├── extensions/                 # Panel ikonu + Ctrl+Space
│   │   └── settings/                   # GSettings override
│   ├── branding/                       # GRUB, Plymouth, GDM, ikonlar
│   ├── systemd/                        # 10 systemd unit + khazar.target
│   ├── iso/                            # ISO builder
│   ├── installer/                      # Setup + first-boot wizard
│   ├── cli/kha                         # İstifadəçi CLI
│   └── rpm/khazar.spec                 # RPM spec
└── docs/                               # Əlavə sənədlər
```

### Build Status

| Komponent | Fayl sayı | Build | Test | Dil |
|-----------|----------|-------|------|-----|
| `sdk` | 14 | 0 error | 22/22 ✅ | C11 |
| `orchestrator` | 20 | 0 error | — | C11 |
| `rule-engine` | 26 | 0 warning | 49/49 ✅ | C11 |
| `policy-engine` | 11 | 0 error | 7/7 ✅ | C11 |
| `model-runtime` | 11 | 0 error | 4/4 ✅ | C11 |
| `intent-classifier` | 7 | 0 error | 12/12 ✅ | C11 |
| `desktop-agent` | 4 | 0 error | — | C11 |
| `package-agent` | 3 | 0 error | — | C11 |
| `network-agent` | 3 | 0 error | — | C11 |
| `power-agent` | 3 | 0 error | — | C11 |
| `audio-agent` | 3 | 0 error | — | C11 |
| **CƏM** | **105** | **0 error** | **94/94 ✅** | |

### Build Komandaları

```bash
make all              # Bütün 11 komponenti qur
make sdk              # Yalnız SDK
make components       # Yalnız server komponentlər
make agents           # Yalnız agentlər
make -C sdk test      # SDK testləri (22)
make -C distro build  # Distro image qur
make install          # Sistemə quraşdır
```

### SDK API (Yeni agent yazmaq üçün)

```c
#include "sdk/include/common.h"      // request_t, response_t, agent_info_t
#include "sdk/logger/logger.h"       // log_info(), log_error(), log_fatal()
#include "sdk/ipc/ipc.h"             // ipc_server_init(), ipc_server_start(), ipc_server_send()
#include "sdk/protocol/protocol.h"   // protocol_build_response(), protocol_parse_request()
```

**Agent yazmaq — 5 dəqiqəlik tutorial:**

```c
#define _GNU_SOURCE
#include "sdk/include/common.h"
#include "sdk/logger/logger.h"
#include "sdk/ipc/ipc.h"
#include <stdio.h>
#include <signal.h>

static volatile bool running = true;

static void handler(int client_fd, const char *data, size_t len, void *ctx) {
    (void)ctx; (void)len;
    char out[4096];

    // Parse request
    request_t req = {0};
    // ... JSON parse ...

    // Execute
    system("firefox");  // əsl agentdə fork+exec

    // Respond
    snprintf(out, sizeof(out),
        "{\"id\":%llu,\"status\":\"success\","
        "\"payload\":{\"message\":\"done\"}}",
        (unsigned long long)req.id);
    ipc_server_send(client_fd, out, strlen(out));
}

int main(void) {
    logger_init(NULL, LOG_INFO);
    signal(SIGINT, [](int){ running = false; });

    // 1. Register with orchestrator
    // ... send JSON registration ...

    // 2. Start IPC server
    ipc_server_t *srv = ipc_server_init("/run/my-agent.sock", 64);
    log_info("agent", "listening on /run/my-agent.sock");

    // 3. Enter event loop
    ipc_server_start(srv, handler, NULL);

    ipc_server_cleanup(srv);
    logger_cleanup();
    return 0;
}
```

> Kompilyasiya: `gcc -std=c11 -O2 my-agent.c -I/path/to/sdk/include -I/path/to/sdk -L/path/to/sdk -lai-sdk -pthread -o my-agent`

---

## GNOME Özəlləşdirmə

KhazarOS GNOME-u tamamilə özünə uyğunlaşdırır:

| Element | Default GNOME | KhazarOS |
|---------|--------------|----------|
| Shell tema | Adwaita | **Khazar Dark** (tünd göy + qırmızı vurğu) |
| GTK4 tema | Adwaita | Khazar Dark |
| İkonlar | Adwaita | Papirus-Dark |
| Panel | Standart | AI Assistant ikonu + "AI" indikatoru |
| Shortcut | — | Ctrl+Space → komanda paneli |
| Divar kağızı | Fedora default | KhazarOS (tünd abstrakt) |
| Boot ekranı | Fedora Plymouth | KhazarOS Plymouth (qırmızı loqo) |
| GRUB | Fedora | KhazarOS (dark theme) |
| Login (GDM) | Fedora | KhazarOS (dark + qırmızı) |
| Şrift | Cantarell | DejaVu Sans + JetBrains Mono |
| Rəng sxemi | Açıq | Tünd (prefer-dark) |

**Rəng palitrası:**

| Rəng | Hex | İstifadə |
|------|-----|---------|
| Tünd göy | `#1a1a2e` | Əsas fon, pəncərə arxa planı |
| Orta göy | `#16213e` | Panel, headerbar |
| Dərin mavi | `#0f3460` | Kartlar, sidebar, vurğu fonu |
| **Khazar qırmızı** | `#e94560` | Vurğu, düymə, seçim, link |
| Açıq mətn | `#e0e0e0` | Əsas mətn |
| Solğun mətn | `#8888aa` | İkinci dərəcəli mətn |

---

## Töhfə

KhazarOS açıq qaynaqdır — istənilən töhfəyə açıqdır.

**Necə töhfə vermək olar:**

1. Fork et
2. Branch yarat: `git checkout -b feature/yeni-agent`
3. Dəyişiklik et
4. Test et: `make -C sdk test`
5. Commit: `git commit -m "feat: yeni agent"`
6. Push + PR aç

**Roadmap:**

| Versiya | Hədəf |
|---------|-------|
| `v0.1` ✅ | SDK + 5 komponent + 5 agent + Policy + GNOME tema |
| `v0.2` | Flatpak agent, GNOME 47 dəstəyi, notification-center |
| `v0.3` | Səs tanıma (Whisper.cpp), Azərbaycan dil modeli |
| `v0.5` | ARM64 (Raspberry Pi 5), enerji optimizasiyası |
| `v1.0` | Stabil buraxılış, LTS dəstək |

---

## Lisensiya

[GNU General Public License v3.0](LICENSE)

Copyright © 2025 Khazar System Distribution

---

<p align="center">
  <sub>Built with C11 • Fedora Silverblue • GNOME • systemd</sub><br>
  <sub>github.com/Khazar-System-Distribution/khazar</sub>
</p>
