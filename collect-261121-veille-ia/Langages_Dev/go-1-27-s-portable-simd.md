---
id: collect-261121-veille-ia/langages-dev/go-1-27-s-portable-simd
title: "Go 1.27's portable SIMD"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["simd", "portable", "some", "tinygo", "code", "models", "read", "back"]
source: https://golangweekly.com/issues/619
source_anchor: ""
source_lines: [1, 33]
section: "Langages_Dev"
---
| #619 — September 25, 2026 | Read the Web Version | 

| 👋 We're taking a break next week, so we'll be back on Friday, October 9. __ *Your editor, Peter Cooper* | 

|  | 

| Together with |  | 

| Go Weekly | 

| Platform-Independent SIMD in Go — SIMD lets a CPU perform an operation on a vector of values at once. Go 1.27 brings experimental SIMD support to arm64 and wasm, plus a portable `simd` package that emulates SIMD where unavailable, so you don't need to write a plain Go fallback. David Chase and Junyang Shao (Go Team) | 

| **RELATED:**  | 

| Ingest and Analytics in One Postgres. No Pipeline — Go compiles to one binary. Your data layer can be just as lean. TimescaleDB extends the Postgres you already run so ingest and analytics live in one database, not a warehouse and a pipeline. Same SQL, one system to run. Get $1000 credit to start. Tiger Data (creators of TimescaleDB) | 

| How Do You Add a New Keyword to Go? (For Sum Types) — Damien's design for sum types is being actively worked through. The latest sticking point is figuring out how to add a `union` keyword without breaking existing code or`gofmt` , with some useful input from Dart's Bob Nystrom. Damien Neil et al. | 

| Is Go's FIPS 140 Code Bloating Your Binaries? — Using `crypto/rand` pulls in extra crypto code even with FIPS off, so WireGuard's creator wants a build-time opt-in. Filippo Valsorda has trimmed some dependencies but argues separate builds would increase testing and maintenance work. Jason A. Donenfeld et al. | 

| **AND MORE:**  | 

| AX: Google's Open Agentic Orchestration Runtime — Designed for large-scale throughput, AX runs AI agents akin to how Kubernetes runs pods. Declare tasks, workspaces, and models in YAML and AX sandboxes the agents and wires it all up. GitHub repo. Jaana Dogan et al. | 

| CodeAF: The Open-Source Software Factory for Open Models — Instead of juggling a terminal for every agent, get one window for every project. Many models, one session: the right one for each call. AgentField.ai | 

| 🔎 Hister: A Go-Powered Search Engine for Your Own Stuff — The creator of SearX (and Colly) is back with a self-hosted personal search engine for re-finding your files or pages you’ve visited. GitHub repo. Adam Tauber | 

| RobotGo 1.1: Native Cross-Platform GUI Automation — Provides an API to control the mouse and keyboard, read the screen, grab screenshots, etc. on macOS, Windows, and Linux. V Caesar et al. | 

|  | 

| The First Official TinyGo Hardware 'Starter Kit' — If you want to do some hardware hacking with TinyGo, this official kit includes a RISC-V board and a variety of modules, along with tutorials for building stuff with it. If you’ve already got some boards or want to source your own, TinyGo also has a list of recommended and supported boards you can use. TinyGo Team and Seeed Studio |
