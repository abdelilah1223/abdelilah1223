import json
from pathlib import Path

data = json.loads(Path("/mnt/data/resume.json").read_text(encoding="utf-8"))
basics = data["basics"]
skills = data["skills"]["technical"]
projects = data["projects"]

# Preserve every skill from the supplied resume, grouped for easier scanning.
groups = {
    "Android & Mobile": [
        "Kotlin", "Jetpack Compose", "Android Views / XML", "Android SDK",
        "Kotlin Coroutines", "Flow", "Room", "DataStore", "Hilt", "WorkManager",
        "CameraX", "MediaStore", "Media3 / ExoPlayer", "HLS / DASH", "FFmpegKit",
        "FFmpeg", "MediaPipe", "ML Kit", "ZXing", "PDFBox / iText7",
        "VpnService API", "Bluetooth HID", "Android Telecom / InCallService",
        "AccessibilityService", "Foreground Services", "WebView / JavaScript Bridge",
        "Storage Access Framework", "Zip4j", "Ktor", "Kotlin Serialization",
        "OkHttp", "Retrofit", "Netty", "Java", "Dart", "Flutter", "React Native", "Expo"
    ],
    "Web & Frontend": [
        "React", "TypeScript", "JavaScript", "Next.js", "Tailwind CSS", "PWA",
        "Chrome Extensions (Manifest V3)"
    ],
    "Backend, APIs & Realtime": [
        "Python", "Flask", "Telethon", "asyncio", "Go (Golang)", "PHP", "Laravel",
        "Node.js", "Express", "GraphQL", "Facebook Graph API", "Socket.IO",
        "Flask-SocketIO", "WebRTC", "PeerJS", "JWT", "OAuth", "TUS Protocol"
    ],
    "Desktop & Systems": [
        "Electron", "Electron Builder", "NeutralinoJS", "Wails", "PyQt6",
        "PySide6", "Tkinter", "Docker", "Git", "Linux Packaging (deb/rpm/AppImage)",
        "systemd", "ADB", "JADX", "Ghidra", "apktool"
    ],
    "Databases": [
        "MySQL", "MariaDB", "MongoDB", "SQLite", "Drizzle ORM"
    ],
    "AI, ML & Computer Vision": [
        "OpenCV", "YOLO", "DeepFace", "face_recognition", "ONNX", "PyTorch",
        "Transformers", "scikit-learn", "NLTK", "LLM Integration",
        "Machine Translation", "Groq API", "Ollama"
    ],
    "Automation & Data Extraction": [
        "crawl4ai", "BeautifulSoup", "yt-dlp", "RSS / Article Extraction",
        "Selenium", "Playwright", "PyInstaller"
    ]
}
grouped = [x for values in groups.values() for x in values]
missing = [x for x in skills if x not in grouped]
# Keep the source list complete even if a future resume skill is not in a category.
if missing:
    groups["Other tools"] = missing

def esc(s):
    return str(s).replace("|", "&#124;").replace("\n", " ").strip()

skill_sections = []
for title, items in groups.items():
    skill_sections.append(f"### {title}\n\n" + " · ".join(f"`{esc(x)}`" for x in items))

project_rows = []
for item in projects:
    name = esc(item["name"])
    link = item.get("link")
    title = f"[**{name}**]({link})" if link else f"**{name}**"
    desc = esc(item.get("description", ""))
    tech = esc(item.get("tech", ""))
    project_rows.append(f"| {title} | {desc} | {tech} |")

experience = []
for job in data["experience"]:
    dates = f'{job.get("startDate","")} – {"Present" if job.get("current") else job.get("endDate","")}'
    experience.append(
        f'**{esc(job["role"])}** — {esc(job["company"])}  \n'
        f'`{dates}`  \n{esc(job.get("highlights",""))}'
    )

education = []
for item in data["education"]:
    dates = f'{item.get("startDate","")} – {item.get("endDate","")}'
    education.append(
        f'**{esc(item["degree"])}**  \n'
        f'{esc(item["institution"])} · `{dates}`  \n'
        f'{esc(item.get("details",""))}'
    )

languages = "\n".join(
    f'| {esc(x["name"])} | {esc(x["level"])} |' for x in data["languages"]
)

readme = f'''<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://capsule-render.vercel.app/api?type=waving&color=0:090014,35:240046,70:3C096C,100:00D9FF&height=230&section=header&text=ABDELILAH%20EL-BAROUDI&fontSize=42&fontColor=F8F7FF&animation=fadeIn&fontAlignY=35&desc=ANDROID%20ENGINEER%20%2F%20FULL-STACK%20%2F%20AI&descAlignY=58&descSize=16">
  <source media="(prefers-color-scheme: light)" srcset="https://capsule-render.vercel.app/api?type=waving&color=0:EEF2FF,35:C4B5FD,70:67E8F9,100:0891B2&height=230&section=header&text=ABDELILAH%20EL-BAROUDI&fontSize=42&fontColor=17102B&animation=fadeIn&fontAlignY=35&desc=ANDROID%20ENGINEER%20%2F%20FULL-STACK%20%2F%20AI&descAlignY=58&descSize=16">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:090014,35:240046,70:3C096C,100:00D9FF&height=230&section=header&text=ABDELILAH%20EL-BAROUDI&fontSize=42&fontColor=F8F7FF&animation=fadeIn&fontAlignY=35&desc=ANDROID%20ENGINEER%20%2F%20FULL-STACK%20%2F%20AI&descAlignY=58&descSize=16" width="100%" alt="Abdelilah El-Baroudi — developer profile">
</picture>

### `// ENGINEERING THE NEXT BUILD`

[![Portfolio](https://img.shields.io/badge/PORTFOLIO-170A2E?style=for-the-badge&logo=googlechrome&logoColor=00E5FF)]({basics["portfolio"]})
[![GitHub](https://img.shields.io/badge/GITHUB-170A2E?style=for-the-badge&logo=github&logoColor=F0E6FF)]({basics["github"]})
[![LinkedIn](https://img.shields.io/badge/LINKEDIN-170A2E?style=for-the-badge&logo=linkedin&logoColor=00E5FF)]({basics["linkedin"]})
[![Email](https://img.shields.io/badge/EMAIL-170A2E?style=for-the-badge&logo=gmail&logoColor=FF4FD8)](mailto:{basics["email"]})

[![Typing SVG](https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=600&size=18&duration=2600&pause=650&color=00D9FF&center=true&vCenter=true&width=760&lines=Native+Android+%2B+platform+engineering;Full-stack+systems+%2B+real-time+apps;AI%2C+automation+%2B+on-device+ML;31+projects+and+still+building...)](https://git.io/typing-svg)

<br>

`abdelilah@dev-station:~$` **build --across mobile, desktop, web & AI**

</div>

---

## `01` — TECHNOLOGY ARSENAL

> **The technologies I work with** — grouped by engineering domain. This section is intentionally near the top so visitors can quickly scan my toolkit.

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://skillicons.dev/icons?i=kotlin,android,java,dart,flutter,react,ts,js,nextjs,python,flask,go,php,laravel,nodejs,express,docker,linux,git,github,mysql,mongodb,sqlite&theme=dark">
  <source media="(prefers-color-scheme: light)" srcset="https://skillicons.dev/icons?i=kotlin,android,java,dart,flutter,react,ts,js,nextjs,python,flask,go,php,laravel,nodejs,express,docker,linux,git,github,mysql,mongodb,sqlite&theme=light">
  <img src="https://skillicons.dev/icons?i=kotlin,android,java,dart,flutter,react,ts,js,nextjs,python,flask,go,php,laravel,nodejs,express,docker,linux,git,github,mysql,mongodb,sqlite&theme=dark" alt="Core technology icons">
</picture>

</div>

<details open>
<summary><strong>View all {len(skills)} technologies and tools</strong></summary>

{chr(10).join(skill_sections)}

</details>

---

## `02` — PROFILE / ABOUT

{esc(data["basics"]["summary"])}

<details>
<summary><strong>Professional strengths</strong></summary>

{chr(10).join("- " + esc(x) for x in data["skills"]["soft"])}

</details>

---

## `03` — SELECTED BUILDS / PROJECTS

**31 projects** across Android, cross-platform mobile, desktop, backend, web, browser extensions, and AI.

| Project | Overview | Stack |
|---|---|---|
{chr(10).join(project_rows)}

---

## `04` — ENGINEERING HIGHLIGHTS

{chr(10).join("- " + esc(x) for x in data["achievements"])}

---

## `05` — GITHUB / ACTIVITY

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-stats.vercel.app/api?username=abdelilah1223&show_icons=true&hide_border=true&bg_color=10051C&title_color=00D9FF&text_color=E9DDFB&icon_color=FF4FD8">
  <source media="(prefers-color-scheme: light)" srcset="https://github-readme-stats.vercel.app/api?username=abdelilah1223&show_icons=true&hide_border=true&bg_color=F3F0FF&title_color=6D28D9&text_color=312E41&icon_color=0891B2">
  <img src="https://github-readme-stats.vercel.app/api?username=abdelilah1223&show_icons=true&hide_border=true&bg_color=10051C&title_color=00D9FF&text_color=E9DDFB&icon_color=FF4FD8" height="175" alt="GitHub statistics">
</picture>
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-stats.vercel.app/api/top-langs/?username=abdelilah1223&layout=compact&hide_border=true&bg_color=10051C&title_color=00D9FF&text_color=E9DDFB">
  <source media="(prefers-color-scheme: light)" srcset="https://github-readme-stats.vercel.app/api/top-langs/?username=abdelilah1223&layout=compact&hide_border=true&bg_color=F3F0FF&title_color=6D28D9&text_color=312E41">
  <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=abdelilah1223&layout=compact&hide_border=true&bg_color=10051C&title_color=00D9FF&text_color=E9DDFB" height="175" alt="Most used languages">
</picture>

</div>

---

## `06` — EXPERIENCE / LOG

{chr(10).join("### " + item for item in experience)}

---

## `07` — EDUCATION / KNOWLEDGE

{chr(10).join("### " + item for item in education)}

---

## `08` — LANGUAGES

| Language | Proficiency |
|---|---|
{languages}

---

## `09` — CONNECT / FIND ME

<div align="center">

[![Portfolio](https://img.shields.io/badge/Portfolio-170A2E?style=for-the-badge&logo=googlechrome&logoColor=00E5FF)]({basics["portfolio"]})
[![LinkedIn](https://img.shields.io/badge/LinkedIn-170A2E?style=for-the-badge&logo=linkedin&logoColor=00E5FF)]({basics["linkedin"]})
[![GitHub](https://img.shields.io/badge/GitHub-170A2E?style=for-the-badge&logo=github&logoColor=F0E6FF)]({basics["github"]})
[![Email](https://img.shields.io/badge/Email-170A2E?style=for-the-badge&logo=gmail&logoColor=FF4FD8)](mailto:{basics["email"]})

**{esc(basics["location"])}**

`// Thanks for visiting. Keep building.`

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://capsule-render.vercel.app/api?type=waving&color=0:090014,35:240046,70:3C096C,100:00D9FF&height=120&section=footer">
  <source media="(prefers-color-scheme: light)" srcset="https://capsule-render.vercel.app/api?type=waving&color=0:EEF2FF,35:C4B5FD,70:67E8F9,100:0891B2&height=120&section=footer">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:090014,35:240046,70:3C096C,100:00D9FF&height=120&section=footer" width="100%" alt="">
</picture>

</div>
'''

out = Path("/mnt/data/README-cyberpunk.md")
out.write_text(readme, encoding="utf-8")
print(f"Created {out} — {len(readme.splitlines())} lines, {len(skills)} skills, {len(projects)} projects.")
