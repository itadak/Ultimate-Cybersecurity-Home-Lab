# Upload the Episode 1 package to the existing GitHub repository

**Existing repository:** https://github.com/itadak/Ultimate-Cybersecurity-Home-Lab

The main root-level `README.md` is **intentionally not included** in this package, because it will be completed when the entire video series is finished. Preserve the existing file.

## Recommended upload with GitHub Desktop

1. Download and unzip this package on your Windows computer.
2. Open [GitHub Desktop](https://desktop.github.com/), sign into your GitHub account, and select **File → Clone repository**. Select your existing `Ultimate-Cybersecurity-Home-Lab` repository.
3. Open the cloned repository's local folder.
4. Copy the package contents — `01-Network-Setup`, `02-pfSense-Firewall`, `03-Kali-Linux`, `04-Ubuntu-Server`, `05-Docker-Portainer`, `VIDEO-01-INDEX.md`, and `.gitignore` — into that local folder. Merge with existing folders, replacing the earlier `01-Network-Setup/README.md` you made. **Do not delete or replace the root `README.md`.**
5. In GitHub Desktop, review the changed files and screenshot previews. Commit with the message `Document Episode 1 lab infrastructure and screenshots`, then click **Push origin**.
6. Refresh the existing repository in your browser. You should see the documentation directories and evidence images.

## Optional command line

If you already have a working local clone, copy these files into it and run:

```bash
git add .gitignore VIDEO-01-INDEX.md 01-Network-Setup 02-pfSense-Firewall 03-Kali-Linux 04-Ubuntu-Server 05-Docker-Portainer
git commit -m "Document Episode 1 lab infrastructure and screenshots"
git push origin main
```

Do not push before reviewing security-sensitive materials.

## Screenshot safety and validation

- The evidence included here is derived from the screenshots provided in this conversation, with some browser areas cropped away; the pfSense device identifier in the console screenshot was masked.
- The package includes **no passwords, API keys, Portainer setup tokens, installer ISO files, or VM disks**.
- Local lab addresses, virtual-machine names and versions are intentionally visible because they substantiate the configuration. Remove or generalize those too if you prefer not to publish network details.
- Confirm pfSense default admin credentials have been changed.
- Confirm the `vmbr1` `10.10.1.0/24` host-address issue, and don't claim VLAN traffic isolation until tested.
- The source screenshots belong to the user's own lab, while the tutorial itself should be attributed to Tech with Gerard.
