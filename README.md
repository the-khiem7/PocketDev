# Elastic AI Development Containers

## Proposal & Repository Vision

## 1. Purpose

Repository này tồn tại với một mục tiêu rất đơn giản:

> Cung cấp các Docker images được build sẵn và một docker-compose stack để chạy môi trường AI coding gồm OpenChamber, OpenCode, 9Router và DevContainer trên các hệ điều hành container-centric như ZimaOS.

Nhiều open-source project không cung cấp Docker image chính thức hoặc image không phù hợp với môi trường container-only. Repository này đóng vai trò **image distribution và compose stack** để có thể pull và chạy toàn bộ hệ thống ngay lập tức.

Repository này **không phải một phần mềm mới**, mà là một **container image collection + runtime stack**.

---

# 2. Goal

Mục tiêu của repository:

* Build Docker images cho:

  * OpenCode
  * OpenChamber
  * 9Router
  * DevContainer CLI / Runner
* Push images lên GHCR
* Cung cấp docker-compose để chạy full stack
* Cho phép chạy trên ZimaOS / homelab / server Docker
* Không yêu cầu host cài toolchain (Node, .NET, Python, Go…)
* Cho phép AI clone repo và chạy project bằng DevContainer

Mục tiêu cuối cùng:

```text
docker compose up -d
```

Và hệ thống AI coding environment chạy ngay.

---

# 3. System Overview

Stack container sẽ gồm:

```text
OpenChamber
OpenCode
9Router
DevContainer Runner
Projects Volume
```

Kiến trúc runtime:

```text
User (Browser / Phone)
        |
   OpenChamber
        |
     OpenCode
        |
      9Router
        |
       LLM
        |
     Projects Folder
        |
 DevContainer (per project)
        |
      Docker
        |
      ZimaOS
```

---

# 4. How the System Is Intended to Work

Workflow dự kiến:

```text
1. User mở OpenChamber
2. User prompt AI tạo project
3. OpenCode clone hoặc tạo repo vào /projects
4. Nếu repo có .devcontainer/devcontainer.json
       → devcontainer up
5. Container project environment start
6. dotnet run / npm run dev chạy trong container
7. Port forward ra ngoài
```

Host không cần cài:

* Node
* .NET
* Python
* Go
* Rust
* Bun
* pnpm
* build tools

Tất cả chạy trong DevContainer.

---

# 5. Repository Responsibilities

Repository này chỉ làm các việc sau:

| Responsibility                   | Description                    |
| -------------------------------- | ------------------------------ |
| Build Docker images              | Từ source open-source projects |
| Publish GHCR images              | Central image registry         |
| Provide docker compose           | Run full stack                 |
| Provide example configs          | Cho OpenCode / provider        |
| Maintain compatibility           | Giữa các container             |
| Provide base runtime environment | Cho AI coding                  |

Repository này **không phát triển OpenCode, OpenChamber hoặc 9Router**, chỉ build image và cung cấp runtime stack.

---

# 6. Container Images

GHCR sẽ chứa các image:

```text
ghcr.io/<owner>/elastic-opencode
ghcr.io/<owner>/elastic-openchamber
ghcr.io/<owner>/elastic-9router
ghcr.io/<owner>/elastic-devcontainer-cli
```

Các image này sẽ được build từ source open-source tương ứng và publish để có thể pull trực tiếp trong docker compose.

---

# 7. Docker Compose Stack

Docker compose sẽ chạy các service sau:

| Service         | Purpose                        |
| --------------- | ------------------------------ |
| openchamber     | Web UI                         |
| opencode        | AI coding agent                |
| router          | LLM router (OpenAI compatible) |
| devcontainer    | DevContainer CLI runner        |
| projects volume | Source code storage            |

Compose stack cho phép:

* AI tạo project
* AI clone repo
* DevContainer chạy project
* Port forward
* Persistent project storage

---

# 8. Directory Structure (Proposed)

Repository structure đề xuất:

```text
elastic-ai-containers/
│
├── images/
│   ├── opencode/
│   ├── openchamber/
│   ├── 9router/
│   └── devcontainer-cli/
│
├── compose/
│   └── docker-compose.yml
│
├── configs/
│   └── opencode.json
│
└── .github/workflows/
    └── build-images.yml
```

---

# 9. Philosophy

Repository này dựa trên các nguyên tắc:

1. Host không cần toolchain
2. Tất cả chạy bằng container
3. AI coding environment phải reproducible
4. Dev environment phải containerized
5. Docker compose phải chạy full stack
6. Images phải có sẵn trên GHCR
7. ZimaOS / homelab phải chạy được
8. DevContainer là runtime environment cho project
9. OpenCode là AI developer
10. OpenChamber là UI

---

# 10. One Sentence Summary

Có thể tóm tắt repository này bằng một câu:

> A container image collection and docker-compose stack for running an AI coding environment with OpenChamber, OpenCode, 9Router and DevContainer on container-only systems like ZimaOS.

---

# 11. Final Concept Diagram

```text
Browser / Phone
       |
  OpenChamber
       |
    OpenCode
       |
     9Router
       |
      LLM
       |
    Projects
       |
 DevContainer
       |
     Docker
       |
     ZimaOS
```