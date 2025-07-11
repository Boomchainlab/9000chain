# 🚀 9000chain

> Autonomous Hash Infrastructure. Designed for iSH iOS. Fueled by $CHONKPUMP9000. Operated by Boomchainlab.

[![9000chain](https://img.shields.io/badge/Boomchain-Powered-blue.svg)](https://boomchainlab.com)
[![GitHub](https://img.shields.io/github/license/Boomchainlab/9000chain)](https://github.com/Boomchainlab/9000chain)

---

**9000chain** is a portable file-integrity auditing CLI designed to generate secure SHA-256 hashes for ZIP files, append them to a transparent `HASHES.md` ledger, and push commits to GitHub — all from your iPhone with [iSH](https://ish.app). It serves as an immutable file notarization layer for Web3.

---

### ✨ Features

- ✅ One-liner setup on iSH for iOS
- 🔐 SHA-256 proof-of-upload log
- 📁 Transparent `HASHES.md` ledger
- ☁️ Auto-commit to GitHub for audit trail
- 🔗 Integrated into CHONKPUMP 9000 ecosystem

---

### ⚙️ One-Line Command (Paste in iSH)

```sh
apk update && apk add --no-cache git unzip bash curl coreutils openssl jq && \
mkdir -p uploads tmp && \
printf "GITHUB_REPO=https://BoomchainLabs:<your_PAT>@github.com/Boomchainlab/9000chain.git" > ~/.env && \
git config --global user.name "9000chain" && \
git config --global user.email "support@boomchainlab.com" && \
[ ! -d 9000chain ] && git clone "$(grep GITHUB_REPO ~/.env | cut -d '=' -f2-)" 9000chain && \
cd 9000chain && mkdir -p uploads tmp && \
for zipfile in $(find uploads -name '*.zip'); do unzip -o "$zipfile" -d tmp/ && HASH=$(sha256sum "$zipfile" | cut -d ' ' -f1) && COMMIT=$(git rev-parse HEAD) && echo "| \`$COMMIT\` | \`$HASH\` |" >> .tmp_hash; done && \
echo -e "| Commit | Hash |\n|--------|------|" > HASHES.md.new && \
cat HASHES.md .tmp_hash 2>/dev/null | grep '|' | sort -u >> HASHES.md.new && \
mv HASHES.md.new HASHES.md && \
git add HASHES.md && \
git commit -m "chore(hashes): update from iSH (9000chain)" || echo "⚠️ No changes" && \
git push --set-upstream origin master || echo "❌ Push failed — check GitHub token" && \
rm -rf tmp .tmp_hash

uploads/*.zip → tmp/ → HASHES.md → git commit → GitHub

---
Replace <username> and <your_PAT> with your actual GitHub credentials.


📂 Folder Structure
9000chain/
├── uploads/            # Place .zip files here
├── tmp/                # Temporary unzip path
├── HASHES.md           # Proof log
└── .env                # GitHub token config


👨‍💻 Maintained by
	•	Org: Boomchainlab
	•	Project: 9000chain
	•	Email: support@boomchainlab.com
	•	Telegram Bot: @Boombot

⸻

📄 License

MIT © Boomchainlab 2025
