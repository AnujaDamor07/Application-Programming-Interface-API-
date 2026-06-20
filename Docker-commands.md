# 🐳 Docker Command Reference

## 📘 Basic Info

#### 🔎 Check Your Installed Docker Version
```bash
docker --version
```

#### 📊 Get Full Details on Your Docker Setup
```bash
docker info
```

#### ❓ Browse Available Commands or Get Help on One Specifically
```bash
docker help
docker help run
```

---

## 🖼 Working With Images

#### 📂 See What Images You've Already Downloaded
```bash
docker images
```

#### ⬇️ Pull an Image from a Registry
```bash
docker pull <image>
docker pull hello-world
docker pull docker.io/kalilinux/kali-rolling
```

---

## 🗑 Removing Images

#### 🔥 Remove One by ID or Name
```bash
docker rmi <image_id>
docker rmi hello-world
```

> ⚠️ **Tip:** stop any container still using that image first, or the removal will fail.
```bash
docker ps -a
docker stop <container_id>
docker rmi <image_name>
```

---

## 📦 Working With Containers

#### ▶️ List Containers That Are Currently Running
```bash
docker ps
```

#### 📋 List Every Container — Running or Stopped
```bash
docker ps -a
docker container list --all
```

---

## 🚀 Creating and Starting Containers

```bash
docker run <image>
docker run hello-world
docker run kalilinux/kali-rolling
```

#### 🔄 Run It in the Background (Detached)
```bash
docker run -d <image>
docker run -d hello-world
```

#### 🖥 Run It Interactively (With a Live Terminal Session)
```bash
docker run -it <image>
docker run -it kalilinux/kali-rolling
```

---

## ⚙️ Managing Container State

#### 🛑 Stop a Running Container
```bash
docker stop <container_id>
docker stop 30f1a2176463
```

#### 🔁 Start a Stopped Container Back Up
```bash
docker start <container_id>
docker start kalilinux/kali-rolling
```

#### 🗑 Remove a Stopped Container
```bash
docker rm <container_id>
docker rm 3b52b6b21464

docker rm -f 4d951b2c1f55      # force remove
docker rm -vf 98490d27bb28     # remove volume + force
```

---

## 📜 Checking Container Logs
```bash
docker logs <container_id>
docker logs a9b1498b2428
```

---

## 🖥 Getting Into a Running Container

#### 🧠 Run a Command Inside It
```bash
docker exec -it <container_id> bash
docker exec -it 78e2eec292dd uname -a
```

> 💡 Genuinely useful when you're working through API pentesting labs — drop into a vulnerable container to poke at its config files, services, and internals directly.

#### 🔗 Attach Directly to a Running Container
```bash
docker attach <container_id>
docker attach 78e2eec292dd
```

> ⚠️ **Tip:** hit `Ctrl + P + Q` to detach without actually killing the container.

---

## 🧹 Cleaning Things Up

#### 🛑 Stop Every Running Container at Once
```bash
docker stop $(docker ps -aq)
```

#### 🗑 Remove a Single Container
```bash
docker rm <container_id>
docker rm -f 4d951b2c1f55        # force remove
docker rm -vf 4d951b2c1f55       # remove volume + force
```

#### 🗑 Remove Specific or All Images
```bash
docker rmi -f ec04eecaa36b
docker images -a
```

#### 🔎 Remove Images Matching a Name Pattern
```bash
docker images -a | grep "pattern" | awk '{print $3}' | xargs docker rmi
```

> 💡 Handy when you've got a pile of lab images with similar names cluttering things up.

---

## 🧼 Pruning Unused Resources

#### 🧹 Clear Out Unused Containers
```bash
docker container prune
```

#### 🖼 Clear Out Unused Images
```bash
docker image prune -f
```

#### 💾 Clear Out Unused Volumes
```bash
docker volume prune -f
```

#### 🌐 Clear Out Unused Networks
```bash
docker network prune
```

---

## 💥 Nuking Everything Unused at Once

This wipes out:
- Stopped containers
- Unused networks
- Dangling images
- The build cache

```bash
docker system prune -a
```

> ⚠️ **Careful with this one** — it removes all unused data system-wide. Fine for a lab environment, risky on anything you actually care about.

For an even more thorough sweep, including volumes:
```bash
docker system prune --volumes
docker system prune --volumes --all
```

---

## 🧠 Quick Reference Table

| 🐳 Command                  | 📖 What It Does           |
| ---------------------------- | --------------------------- |
| `docker ps`                  | List running containers     |
| `docker images`               | List images                  |
| `docker exec -it <id> bash`   | Open a shell inside a container |
| `docker stop <id>`            | Stop a container             |
| `docker start <id>`           | Start a container             |
| `docker rm <id>`              | Remove a container            |
| `docker rmi <id>`             | Remove an image               |
| `docker system prune -a`      | Clean up everything unused    |
