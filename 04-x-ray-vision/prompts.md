# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.
Open the folder 00-rook/code/dispatch-routing/. This is the part of our software that decides who gets asked to take a job. I have never read code before and I am not going to start now. Walk me through what happens from the moment something goes wrong somewhere to the moment a responder's phone buzzes, in plain English, no jargon. Then tell me which file each step lives in.

### 2.
yes, but before we do that, can you tell me more about anything that has changed in the code since the 4.2 rollout? recently?

### 3.
ok, so no other changes?  Can you detect any bugs in the code?

### 4.
Does this change the fix recommendations for a 4.2 patch or 4.3 build?

### 5.
So can we generally conclude that 4.2 is working as designed but it has created a scenario where 4 people have seen a consistent downward trend and that this will not automatically resolve itself without intervention?

### 6.
ok, please bullet list the key issues that could cause problems with 4.2?

### 7.
please summarize these into one sentence.

### 8.
Moving onto a new line of thinking.  We have a chat from Marcus in engineering where he asks:  "did the change to who gets asked first apply to responders who'd already been turning jobs down, or just new ones? Can't tell from the code and don't want to guess on this. Anyone know?"  Can we compare the files from the last session to the codebase to see if it backs up what we have previously concluded?  Does it answer Marcus's question?

### 9.
yes, draft the reply to Marcus

### 10.
Please find me the part of this code that removes points when someone misses a ping or turns one down.  Show it to me and explain it in plain english.  Then please find me every single thing in this code that adds points to somebody's points total.

### 11.
What about the theory about potential bugs or a reset that could have happened?

### 12.
Somebody has been quiet for a month. Walk me through, step by step, exactly what would have to happen for them to start getting work again.

### 13.
Makes sense.  So the code, while potentially not fairly optimized, does look to be sound?

### 14.
is there anything in the data that points to quality of delivery?
