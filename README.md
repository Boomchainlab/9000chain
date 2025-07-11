# 9000chain
iSH-powered CLI pipeline for hashing, verifying, and committing uploads to GitHub in the Boomchain ecosystem.


# 🔗 9000chain

A minimal Git-based hashing and verification pipeline designed for **iSH on iOS** and the Boomchain infrastructure.  
Track file uploads, generate verifiable SHA-256 hashes, and commit the results directly to GitHub using secure automation.

---

## 🚀 Features

- 🧩 Auto-detect `.zip` uploads
- 🔐 SHA-256 hashing & Git-based ledger
- 📲 iSH iOS compatible
- 🔁 GitHub push with PAT authentication
- 📤 Works well with Boomchain projects (e.g. `CHONKPUMP 9000`, `contact-keys`)

---

## 📁 Upload Flow

uploads/*.zip → tmp/ → HASHES.md → git commit → GitHub

---

## 🧰 Linear iSH Installation Script

```sh
apk update && apk add --no-cache git unzip bash curl coreutils openssl jq && \
mkdir -p uploads tmp && \
printf "GITHUB_REPO=https://<username>:<your_PAT>@github.com/Boomchainlab/9000chain.git" > ~/.env && \
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


Replace <username> and <your_PAT> with your actual GitHub credentials.

👨‍💻 Maintained by
	•	Org: Boomchainlab
	•	Project: 9000chain
	•	Email: support@boomchainlab.com
	•	Telegram Bot: @Boombot

⸻

📄 License

MIT © Boomchainlab
