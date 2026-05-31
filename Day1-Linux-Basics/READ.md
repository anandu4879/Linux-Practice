# Day 01 — Getting Comfortable with Linux

So today I started properly getting into Linux. I've heard the name a thousand
times but never really sat down to understand what it actually is. Turns out
there's a lot more to it than just "the thing programmers use."

---

## What even is Linux?

Linux isn't really an operating system by itself — it's a **kernel**. Think of
the kernel as the core engine that talks to your hardware. What we call "Linux"
(like Ubuntu or Fedora) is actually the kernel bundled together with a bunch of
tools, a package manager, and sometimes a desktop environment.

Linus Torvalds wrote the kernel back in 1991 as a hobby project. That hobby
project now runs most of the internet, basically every Android phone, and almost
every server on the planet. Not bad.

---

## The distro situation

One thing that confused me at first — why are there so many versions of Linux?
They're called **distributions** (distros), and each one is basically a
different opinion on how Linux should be packaged and used.

Here's what I figured out about the main ones:

- **Ubuntu** — most beginner-friendly, huge community, good docs everywhere
- **Fedora** — slightly more cutting-edge, popular with developers
- **Arch** — you build it yourself from scratch, total control, steep learning curve
- **Kali** — comes pre-loaded with security/hacking tools
- **CentOS / RHEL** — what you'll find on enterprise servers at companies

For now I'm sticking with Ubuntu. No reason to make this harder than it needs to be.

---

## Terminal stuff I practiced today

This is the part that actually took some time. The terminal feels weird at first
because there's no undo button and everything is very literal. But once it
clicks, it's genuinely faster than clicking around in a file manager.

```bash
# figuring out where I am
pwd

# seeing what's in a folder (-l for details, -a to show hidden files)
ls -la

# moving around
cd /home/user/Documents
cd ..   # go back one level
cd ~    # go back to home directory

# creating stuff
touch notes.txt        # creates an empty file
mkdir my-project       # creates a folder

# copying, moving, deleting
cp notes.txt backup.txt
mv backup.txt my-project/
rm my-project/backup.txt   # no recycle bin, it's just gone

# reading files
cat notes.txt          # dumps the whole thing
less notes.txt         # lets you scroll through it
head -n 5 notes.txt    # first 5 lines
tail -n 5 notes.txt    # last 5 lines
```

The one that got me was `rm`. There's no "are you sure?" — it just deletes.
Lesson learned before I made a mistake, thankfully.

---

## Permissions — this one took a while

When you do `ls -l` you see something like `-rwxr-xr-x` next to every file.
That's the permission string and it looks scary but it's actually pretty logical
once you break it down.

rwx  r-x  r-x
↑   ↑    ↑    ↑
│   │    │    └─ everyone else (others)
│   │    └────── people in the same group
│   └─────────── the file's owner
└─────────────── file type (- = file, d = directory)

Each `rwx` means read, write, execute. A `-` means that permission is off.

To change permissions you use `chmod`. I practiced with:

```bash
chmod 755 script.sh   # owner can do everything, others can read and run
chmod 644 notes.txt   # owner can read/write, others can only read
```

The numbers are actually just a shorthand — each digit is a combination of
read(4) + write(2) + execute(1). So 7 = 4+2+1 = full access. Once I saw that
it made way more sense.
