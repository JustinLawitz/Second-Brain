# Second-Brain

Overview:
Over my time at UTSA and Alterman I’ve discovered I both enjoy and greatly benefit from writing notes and making my own documentation on whatever I may be working on, weather project, or classwork. One of reasons I created a home server/NAS was to hold notes and documentation in one central place. I also wanted a disaster recovery system in place in case of the degradation of any disk a note/document might be on. Also, over the past couple years my own and essentially the entire worlds workflow has changed drastically due to AI. Along with the goals of centralizing my notes and implementing disaster recovery, I also wanted to tie my agent of choice (Claude) into this system to reference my notes when working on projects, saving me a lot of time and headache explaining things I’ve already done, and giving it context on a situation.  This project ended up being split into multiple different branches involving different software’s, extensions, and containers (many of which I had never worked with before). To summarize a bit about each section of the project and what each does, Obsidian (the vault itself) holds the actual notes as plain markdown files on my PC’s disk. This is the source of truth for all other sections. The folder structure contained in Obsidian is an organizational layer and does not affect functionality. It just makes the notes easier for me (and Claude) to navigate. Gitea and git handle version control and work as a remote copy running on my homelab server in case of an accidental deletion, degradation of my PC’s drive, etc. ZFS and snapshots handled general disaster recovery for the server itself (Gitea LXC) and is independent of git. The Obsidian Git plugin automated commits/pushes to Gitea on a timer instead of running these commands manually on a terminal. And finally, the Claude Desktop Filesystem extension created the bridge between Claude and Obsidian, allowing it to read and query the vault directly. 

## Step 1.

The first thing I did was install obsidian at obsidian.md/downloads (Figure 1), which is a powerful, local first note taking and knowledge management app. Once downloaded, I ran the exe and selected the location I wanted to install it to (D:\Obsidian\)

<img width="975" height="521" alt="image" src="https://github.com/user-attachments/assets/9e403706-a646-4b14-b603-b78a850d0e49" />

Figure 1: Obsidian download

## Step 2.

On the launch screen I selected Create new vault, and named it second-brain along with creating a subfolder in the Obsidian folder for this vault to be kept in (Figure 2).

<img width="975" height="788" alt="image" src="https://github.com/user-attachments/assets/679d954a-566b-47d4-b025-ff9693adb3db" />

Figure 2: second-brain vault creation

## Step 3.

I next wanted to add some Community plugins to the vault under Settings > Community plugins > Browse, I installed and enabled Templater for consistent structure on every new note, Dataview to query notes like a database once they’re tagged, and Tag Wrangler to rename or merge tags as the taxonomy evolves (Figure 3).

<img width="975" height="711" alt="image" src="https://github.com/user-attachments/assets/6533d139-10e4-4080-a21f-6c4b4da01229" />

Figure 3: Community plugin example

## Step 4.

Once these plugins were added, I started to decide on how the skeleton of the folders should work. I decided on 5 separate folders, as this vault would hold and reference documents from my recent internship with Alterman, class work, and personal projects. The first folder is “00-inbox” for raw, daily dumps like the technical notes I created at Alterman, it will essentially be for pre-processed notes. Then “01-projects” with active, ongoing work with a defined outcome, mainly for personal projects. “02-area” will be for outstanding responsibilities with no end date, such as findings based on my own Unifi home infrastructure for example. “03-resources” which will be for straight forward references and gotchas such as networking quirks found at home or Alterman, and documentation made for class work. And finally “04-archive” which will be used for completed documents such as those finished at Alterman (Figure 4).

<img width="975" height="762" alt="image" src="https://github.com/user-attachments/assets/2715af24-4096-4a6a-8044-7bd6f386c6cd" />

Figure 4: Folder skeleton

## Step 5.

After the basic structure was completed, I started by first adding the documentation I created on projects I did at Alterman, splitting the 3 documents I had worked with ona  daily basis into different markdown files, such as Daily Log and Tech Notes for the raw data I collected, and Master Record as essentially a refined combination of the raw data. I placed these 3 files under 04-archive/alterman-internship. I then separated the technical gotchas out from the raw data and created 5 md files and placed them under 03-resources/tech-references. The files contained learned gotchas for Certs and PKI, Hardware, ManageEngine, Networking, and Process (Figure 5). I also read about and implemented the syntax used by Obsidian to add tags to files and make connections or relationships between them. The properties I decided to use were type, tags, and sometimes status depending on the file. The syntax for this was to use --- type: type tags: [tag,tag,…] status: status ---. The three dashes created the properties panel and created fields for the included values. Clicking on a tag also listed all other files containing this tag. To create a link to another file, the syntax was simply [[file name]]. This created a clickable link to the listed file and created a connection in the graph view (Figure 6).

<img width="975" height="765" alt="image" src="https://github.com/user-attachments/assets/49cd3fed-3bd8-4303-aee7-412b79328d56" />

Figure 5: md file and syntax example

<img width="975" height="761" alt="image" src="https://github.com/user-attachments/assets/10e389aa-42ff-4482-bf79-0d8700e930ab" />

Figure 6: Graph view example

## Step 6.

After adding the documentation I made at Alterman, I next converted the documentation from my personal projects from docx to md and added it to the vault under 01-projects/homelab, creating 4 separate md files with one for each project so far. I also appended the gotchas I discovered in these projects to the related md files in tech-references. Specifically, the Networking and Hardware files. I also made sure to add the relations to the relevant tech references and other home lab projects (Figure 7).

<img width="975" height="764" alt="image" src="https://github.com/user-attachments/assets/c7a8478b-4c8a-4b80-82a2-f2e3aec03784" />

Figure 7: Added home lab project documentation

## Step 7.

The next set of md files I added was from a Telecom and Networking course. I turned each chapters notes into md files (11 chapters) and made an md file that combined all 11 chapters notes. I then uploaded all 12 files to the vault and related each chapter to the general file including all chapters notes.

<img width="975" height="759" alt="image" src="https://github.com/user-attachments/assets/2021f6aa-f2d8-40d8-a296-5c128237a873" />

Figure 8: Graph view including course notes

## Step 8.

For now I’m happy with the documentation I added to the obsidian vault, so I’ll begin working with ProxMox on the server I made in my previous lab, The first thing I did was connect to it at https://192.168.22.92:8006 and login (Figure 9).

<img width="975" height="680" alt="image" src="https://github.com/user-attachments/assets/f80af2e4-0524-44b4-a2ba-9b6c73a9dafe" />

Figure 9: ProxMox web UI

## Step 9. 

In ProxMox I’ll be running Gitea on an LXC (Linux Container). In comparison to a VM, a container shares the host’s kernel and just isolates processes, file systems, and networking on top of it. It’s essentially a fenced off space on the same OS. LXC are best for single purpose Linux services, with the advantage of a near instant boot time, minimal RAM and disk overhead due to no duplicate kernel or emulated hardware. Isolation is weaker than a VM though and its possible for a process inside a container to escape and reach the host kernel. To go a bit more into Gitea, it is a self-hosted Git server which essentially gives you your own private, lightweight web UI to browse repos, Git push/pull over HTTP or SSH, user accounts and repo permissions, etc. To create the LXC in Proxmox I selected pve01 > local (pve01 storage > CT Templates tab > Templates button > and due to a good amount of experience with ubuntu, deciding on ubuntu-26.04 > Download (Figure 10).

<img width="975" height="680" alt="image" src="https://github.com/user-attachments/assets/b9959871-eb14-4412-ac67-72df20e9e166" />

Figure 10: Templates menu

## Step 10. 

After Ubuntu finished downloading, I selected Create CT top right and in the General tab added the Hostname “gitea” and created a root password for the container (Figure 11). In the Template tab I selected ubuntu-26.04. Under the Disks menu I changed the Disk size to 16 GiB which should be plenty of room for Gitea and the note vault, I also changed the Cores in use under CPU to 2 instead of 1.  Under the Memory tab I changed both Memory and Swap to use 1024 MiB. Swap will only be used if the container’s actual RAM usage exceeds the initial 1024 MiB. This will rarely if ever happen as 1024 MiB is plenty for Gitea. The Network tab is one of the most important here as I had to set a static IP so the address doesn’t change and break the git remote later. After a quick scroll through the Unifi web UI I decided on the IP 192.168.22.88 on the private VLAN. I also added the gateways IP on the Network tab (Figure 12). The DNS tab defaults to using host settings which already uses the Pi-Hole I set up, so I left this blank, I then made sure everything was correct in the Confirm tab and clicked Finish after making sure to tick the Start after created box to boot the container.

<img width="823" height="613" alt="image" src="https://github.com/user-attachments/assets/ae77ae32-38c3-4137-b568-84c27fe13710" />

Figure 11: Create CT General tab

<img width="829" height="620" alt="image" src="https://github.com/user-attachments/assets/7d79cf67-9662-4f7c-9c20-463470943b41" />

Figure 12: Create CT Network tab

## Step 11.

I left the container to run overnight to monitor, and while idle it used a fraction of the allocated CPU cores, Memory, and Disk. Because of this I did not change any of the metrics I had set prior (Figure 13)

<img width="975" height="679" alt="image" src="https://github.com/user-attachments/assets/84cbc19b-773f-4007-a5df-81f91a28ad3a" />

Figure 13: gitea container under pve01

## Step 12.

Once entering the console of the Gitea LXC (top right Console button), I first ran some housekeeping commands such as “# apt upgrade -y”, “#apt upgrade -y”, and “# apt install -y curl” to make sure everything was up to date and install curl as base level Ubuntu does not come with it. I then ran the commands “# curl -fsSL https://get.docker.com -o get-docker.sh” and “# sh get-docker.sh” to install Docker. Docker is a tool for packaging an application together with everything it needs to run like its dependencies, runtime, libraries, config, etc. Docker packs all of this into a container image that runs and behaves in the same way regardless of what underneat such as physical hardware, this LXC, a cloud server, etc. Docker is an incredibly useful tool because instead of installing Gitea’s exact runtime dependencies, setting up a database, configuring paths, and hoping nothing conflicts with anything else, all we have to do is write a config file and Docker handles the rest and pulls the correct pre-built environment while running it consistently. The language can also be a bit confusing because the “gitea” we’re working on is also technically a container itself. Essentially the LXC is a lightweight Ubuntu VM. Docker containers are completely different and scoped much smaller running inside the LXC on top of the Ubuntu OS. Once we bring up Gitea using Docker, it will be in its own Docker container running just the Gitea process and nothing else (Figure 14). I also installed docker compose using “# apt install -y docker-compose-plugin” which is used to define and run multi-part Docker setups using a config file instead of typing long docker run commands by hand. 

<img width="975" height="581" alt="image" src="https://github.com/user-attachments/assets/a7ca30a1-7ace-43d5-abbd-0076657c132e" />

Figure 14: Docker installation

## Step 13.

After installing Docker and Docker compose, I ran “# systemctl enable –now docker” which starts Docker now and makes it launch automatically every time the LXC boots. I then ran “# docker run hello-world” to confirm Docker is running correctly and can pull and run images (Figure 15).

<img width="975" height="588" alt="image" src="https://github.com/user-attachments/assets/5d062e60-08b8-45a9-88b0-319df24e5eb6" />

Figure 15: Successful pull and run of hello-world image

## Step 14.

I then made a directory using “# mkdir -p /opt/gitea/data”, and connected to it using “# cd /opt/gitea”. This is where Gitea’s Docker Compose config and all its persistent data like repos and databases will live. Once the directory was created I ran “# nano docker-compose.yml” to write a yml file inside the directory, and wrote the compose file below, saving with Ctrl + o. I also confirmed it saved using “# cat docker-compose.yml” inside the gitea directory (Figure 16).

<img width="975" height="588" alt="image" src="https://github.com/user-attachments/assets/c6665b02-e94e-4067-ab3b-47d93f99fdba" />

Figure 16: Docker Compose yml file

## Step 15.

After creating the Docker Compose file, I pulled it using “# docker compose up -d”, and navigated to http://192.168.22.88:3000 (port set in compose file) (Figure 17).

<img width="975" height="680" alt="image" src="https://github.com/user-attachments/assets/1b32ba2a-3988-49bd-8e2b-7b7d395f44a8" />

Figure 17: http://192.168.22.88:3000

## Step 16.

Since all the database settings matched what we wrote in the config file, under Optional Settings > Administrator Account Settings, I created a username, added my Email, and created a password. I then clicked Install Gitea. After a couple seconds I was redirected to the Gitea home page (Figure 18).

<img width="975" height="684" alt="image" src="https://github.com/user-attachments/assets/8b7499c0-1562-4177-a60a-166f77abd8dc" />

Figure 18: Gitea home page

## Step 17.

On the home page I clicked Create (top right +) > New Repository. I made the Repository Name “second-brain”, and made sure to tick the Make repository private box under visibility as some of the notes I have in the obsidian vault hold private information like names and IPs. I also confirmed the Initialize Repository (Adds .gitgnore, License and README) was unchecked since the plan is to push the existing vault with its own history. I then clicked Create Repository (Figure 19).

<img width="975" height="684" alt="image" src="https://github.com/user-attachments/assets/371ba0e9-cfb2-4315-b3d9-5a36beb8af5d" />

Figure 19: second-brain repo settings

## Step 18.

I then opened Obsidian, right clicked on the vault name (second-brain) bottom left, and selected Show in system explorer. This lead me to the file path I had set up earlier, being D:\Obsidian\second-brain\second-brain. In this file path I added a new text document and named it .gitignore and removing the .txt file extension (Figure 20), I confirmed this worked by looking under Type and seeing it classified as a “GITIGNORE File” type.

<img width="975" height="761" alt="image" src="https://github.com/user-attachments/assets/fe787652-a0f0-4cc9-8531-4bc38e4a9107" />

Figure 20:.gitignore file creation

## Step 19.

After creating the file, I opened it in notepad and added a list of files to it (Figure 21). The purpose of this file is to essentially tell git to not track changes to these files/folders, and pretend they don’t exist. Without this file, git would grab everything in the vault folder, including files that change constantly just from using Obsidian, no just any documentation I would add or change.

<img width="958" height="391" alt="image" src="https://github.com/user-attachments/assets/2a6ba286-a35d-43f4-bef8-07eede70d0b8" />

Figure 21: List of files/folders in gitignore

## Step 20.

Next, I needed to install Git on my PC holding the vault itself. I did this at https://git-scm.com/install/windows, and downloaded the latest version(2.55.0(5)) x64). After running the .exe, the default options for the setup were all correct. I double checked to make sure that Git from the command line and also from 3rd-party software was selected under Adjusting your PATH environment so git would work in the command line (Figure 22). After the installation had finished, in the vaults file path in File Explorer I entered cmd and first ran “# git –version” to confirm the installation. After that I ran the set of commands “# git init”, “# git add .gitignore”, “# git add .”, “# git config --global user.email justinblawitz@gmail.com”, “# git config --global user.name "Justin Lawitz"”, “# git commit -m "Initial vault commit"”, “# git branch -M main”, “# git remote add origin http://192.168.22.88:3000/JustinLawitz/second-brain.git”, and  “# git push -u origin main”. What this set of commands does is turn the vault folder into a git repository by creating a hidden .git folder inside of it, and stages the gitignore file, telling git to include it in the next commit. It then stages everything else in the folder other than whatever is in gitignore for the next commit. After that it takes everything staged and lick it in as your permeant snapshot in the repo’s history, and renames the default branch to main. I then essentially told git there’s a copy of this repo living at this URL, and it is called origin, and to upload the commit and all the files in it to origin (the remote Gitea repo. It also sets origin main as the default target for future pushes. After running this set of commands a Gitea window opened to confirm my credentials and authorize “Git Credential Manager” to access my account (Figure 23).

<img width="925" height="711" alt="image" src="https://github.com/user-attachments/assets/1f68d121-5164-467c-b328-26b5fd6aea11" />

Figure 22: Git setup

<img width="975" height="353" alt="image" src="https://github.com/user-attachments/assets/4881a385-ae71-4e3f-bcbd-09756fc17a46" />

Figure 23: CLI and Git credential manager authorization

## Step 21.

After returning to http://192.168.22.88:3000/JustinLawitz/second-brain and refreshing the page, I saw the full folder tree instead of the “Quick Guide” . I also confirmed the actual .md files themselves looked correct (Figure 24).

<img width="975" height="683" alt="image" src="https://github.com/user-attachments/assets/66947f1b-7c7b-452c-9082-87559facd4ba" />

Figure 24: .md files successfully displayed in Gitea

## Step 22.

The next step is to create a ZFS snapshot of the LXC we’ve been working on. A snapshot is a read only, point in time copy of a filesystems’s state. It’s taken instantly and costs almost no disk space. This is possibly because of copy on write, which doesn’t duplicate data, but remembers “this is what block X looked like at time T”. As you modify files, the snapshot keeps the old blocks around instead of letting them get overwritten. So the space cost only grows as data actually changes. This matters because git protects from errors like bad edits, and deleted notes, along with full history and easy rollback. The ZFS snapshots protect us from things that git doesn’t cover like an incorrect docker compose command, a corrupted SQLite database, an rm -rf in the wrong directory, or even the whole Gitea container getting destroyed. The one catch with my current ProxMox setup is that the Gitea LXC’s root disk is sitting on the NVMe SSD running ProxMox itself, not on the ZFS mirror pool on my two 8TB HDDs. To fix this I have to move Gitea’s actual data at /opt/gitea/data into the ZFS pool using a bind mount, which makes a folder on a disk visible at two different paths at once. To do this I first opened the shell for pve01 (Shell button top right), and ran the commands “# zfs create storage/gitea-data” to add Gitea’s data to the ZFS pool, and confirmed it using “# zfs list” and viewing storage/gitea-data (Figure 25).

<img width="975" height="679" alt="image" src="https://github.com/user-attachments/assets/679ed356-e19f-4660-83bd-ecc28b297225" />

Figure 25: Gitea bind mount

## Step 23.

To create the mount point I first temporarily shut down the container, then I went to the Resources tab > Add > Mount Point. Here I added the path (/mnt/gitea-data), selected the ZFS pool under Storage, and changed the Disk size to 32 GiB, then selected Create (Figure 26). After successful creation, I restarted the container.

<img width="975" height="681" alt="image" src="https://github.com/user-attachments/assets/a0eab8a4-ace5-45f7-80d9-c0962ab113b7" />

Figure 26: Mount Point for Gitea

## Step 24.

After adding the mount point, I next needed to copy the data over to this new mount point. To do this I entered the gitea LXCs console, and ran “# cd /opt/gitea”, “# docker compose down”, and “# cp -a /opt/gitea/data/. /mnt/gitea-data/.” To cleanly stop Gitea, and then copy the files over to the mount point. I then ran “# nano docker-compose.yml” and changed the line reading “- ./data:/data” to “- /mnt/gitea-data:/data” and saved using Ctrl +o (Figure 27). I then ran “# cat docker-compose.yml” to confirm the change was saved, and brought the compose file back up using “# docker compose up -d”. I then quickly confirmed everything on Gitea’s side was still intact by looking through the folders at  http://192.168.22.88:3000. I also double checked in the console for the container by running “# due -sh /mnt/gitea-data” and confirming it was a reasonable size, and running “ls -la /mnt/gitea data” to make sure the files/folders looked correct on the mount. I was then able to remove the stale copy on the NVMe by running “# rm -rf /opt/gitea/data”.

<img width="975" height="379" alt="image" src="https://github.com/user-attachments/assets/251bc1c1-9470-4adb-9f7b-cda02535a24c" />

Figure 27: Edited compose file

## Step 25.

To begin taking automatic ZFS snapshots of the container I went back to the pve01 shell and ran “# apt install zfs-auto-snapshot”. This package handles creating snapshots on a schedule and pruning old ones automatically. After it had installed I confirmed its scheduled ZFS snapshots using “# ls /etc/cron.d | grep zfs”, and “# cat /etc/cron.hourly/zfs-auto-snapshot 2>/dev/null || ls /etc/cron.*/zfs*” (Figure 28). These commands check for scheduled cron jobs system wide and lists anything containing “zfs”, and checks for the contents of the hourly snapshot script.

<img width="975" height="602" alt="image" src="https://github.com/user-attachments/assets/b6ff119d-816b-492d-aa0e-20954435d4cb" />

Figure 28: Auto ZFS snapshot confirmation

## Step 26.

I let the ZFS automatic snapshots run for about 24 hours. I entered the shell on pve01 to check if the snapshot system was working along with how much space it was using. The first command I used was “# zfs list -t snapshot” which listed out the snapshots taken overnight and throughout the day. The second command I used was “# zfs list -o space storage/gitea-data” which listed the storage that had been used by these snapshots (Figure 29). The first command’s output showed the list of snapshots, nearly all of them with 0B used. This confirmed that if nothing changed in the vault itself, the snapshot would not consume storage because there was nothing new to save. Another interesting thing about the snapshots was their frequency, snapshots were being taken at a “frequent” (15 minutes), “hourly”, “daily”, and “weekly: intervals. This is for granularity, as if an error was made 15-20 minutes again, I wouldn’t need to roll back an entire day of work, but just the latest 15-30 minutes. There is no down side to this because the snapshots would only save new data, so the influx of snapshots did not hinder the system at all. The second command also displayed gitea-data had only used 96K of data.

<img width="975" height="589" alt="image" src="https://github.com/user-attachments/assets/974c1cb1-3e31-44b6-a3ea-6d34455e3254" />

Figure 29: Output of snapshot commands

## Step 27.

Since the ZFS snapshots were confirmed to be working, the next step involved installing a plugin to Obsidian to automatically push changes to Git. Opening Obsidian I went to Settings > Community plugins > made sure “Restricted mode” was off > Browse > Search and select the “Git” plugin > Install and Enable (Figure 30). Back in Obsidian settings under Community plugins at the bottom, I selected Git and changed Auto commit-and-sync interval (minutes) to 15, and enabled Auto commit-and-sync after latest commit. This second setting makes the timer reset not just based on the last automatic commit, but the last actual commit. So if I push a manual commit with 3 minutes left on the 15-minute timer, it will reset instead of committing again in 4 minutes. This is helpful to stop redundant commits and keeps commit logs cleaner. I tested this by adding some text to the bottom of a note (“Testing auto commit”) in Obsidian and waiting for the automatic commit. After a couple minutes the test displayed on the note in Gitea (Figure 31). I tested a manual sync by deleting the test string, pressing Ctrl + P, and selecting “Commit and sync: which would automatically sync any changes instead of waiting the full timer. This successfully removed the test string from the note in Gitea.

<img width="864" height="627" alt="image" src="https://github.com/user-attachments/assets/c55c7510-08bf-4249-b1ba-53f310e31618" />

Figure 30: Git plugin by Vinzent

<img width="880" height="613" alt="image" src="https://github.com/user-attachments/assets/2f57b348-affc-4df3-8840-1604e925d9c0" />

Figure 31: Working auto commit

## Step 28.

The final step was to integrate the vault into Claude. I did this by opening Claude Desktop > Settings > Extensions > Browse extensions > searching and installing the “Filesystem” extension, making sure to enable it afterwards. After restarting Claude desktop and opening a new conversation, I confirmed the extension was on by clicking + > hovering Connectors, and making sure Filesystem was on. I also had to set the permissions of the extension itself under Settings > Extensions > Configure Filesystem > and editing Tool permissions. I enabled all the read functions and disabled all the write/delete functions other than copy file to Claude. I then went back to the new chat and asked it a question It would need access to the vault to answer, which it received successfully (Figure 32). 

<img width="975" height="492" alt="image" src="https://github.com/user-attachments/assets/2cadbed7-2e9e-4cc2-8efb-6f859b3af3b9" />

Figure 32: Successful data pull from second-brain
