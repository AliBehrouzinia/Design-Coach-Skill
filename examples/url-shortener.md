# Example: designing a URL shortener

> This is an illustrative conversation, written to show how the coach works. It is shortened, but the flow and the style of questions are what you'll see in practice.

---

**You:** I want to design a URL shortener for our team. Can you help?

**Coach:** *(Problem)* Sure. Before any design: in one or two sentences, what problem does it solve, and for whom?

**You:** Our marketing team shares long tracking links in posts and emails. They want short links, and they want to see how many clicks each one gets.

**Coach:** So two jobs: short links, and click counts. *(Goals & constraints)* Roughly how many links will be created, and how many clicks do you expect on a busy day?

**You:** Maybe a few hundred links a month. Clicks... no idea. Maybe a campaign gets 50,000 in a day?

**Coach:** Good enough to start. What must it *never* do? Think about what would embarrass the team.

**You:** A short link going to the wrong page. Or links breaking after a while.

**Coach:** Those are strong constraints: correctness and durability. *(Unknowns)* Which of your assumptions would hurt most if it turned out wrong?

**You:** The 50,000 number, I guess. If a post goes viral it could be way more.

**Coach:** Keep that in mind for the stress test. *(Options)* Before we pick anything: how could you turn a long URL into a short code? Try to give me at least two different ways.

**You:** Hash the URL and take the first few characters. Or... I'm not sure what else.

**Coach:** Hint: forget the URL for a second. Is there something else that's already unique for every new link you store?

**You:** Oh, the database ID. I could convert the auto-increment ID to base62.

**Coach:** Two options. *(Trade-offs)* What happens with each one when two different URLs produce the same code? And what does each one reveal to someone looking at the codes?

**You:** Hashes can collide, so I'd need to check and retry. IDs never collide, but the codes are sequential, so anyone can guess other links and see how many we've made.

**Coach:** Both real costs. Given your constraints, which one do you prefer, and how would you explain the choice to a skeptical colleague?

**You:** The ID approach. "Never points to the wrong page" matters most, and these are marketing links, they're public anyway. Guessable codes are fine for us.

**Coach:** Clear reasoning, and you tied it back to your own constraint. *(Design)* Let's split it into parts. What are the main pieces, and what does each one own?

**You:** An API to create links, a redirect service that looks up the code, a database with links, and something that counts clicks.

**Coach:** Take the redirect path first, since it's the hot path. What happens, step by step, when someone clicks a link?

**You:** Look up the code in the database, increment the click count, return a redirect.

**Coach:** *(Stress test)* Your viral post arrives: 2,000 clicks per second on one link. Walk me through what happens to that "increment the click count" step.

**You:** Everyone's updating the same row... that's going to lock up. Maybe I should write click events to a queue or a log and count them separately, so the redirect doesn't wait.

**Coach:** What did that change cost you?

**You:** The counts aren't real-time anymore. But marketing checks them once a day, so a few minutes of delay is fine.

**Coach:** Good trade, and you checked it against a real need. Want to keep going at this level of detail, or speed up?

**You:** Let's wrap up.

**Coach:** *(Summary)* Then summarize the design and the key decisions in your own words.

**You:** Base62 codes from database IDs, because correctness matters more than hiding link counts. Redirects only read from the database. Clicks go to a log and are counted asynchronously, so a viral link can't slow down redirects.

**Coach:** That's a solid design, and every decision has a reason you can defend. One thing you didn't cover: durability. You said links must never break. What happens if the database is lost? Worth thinking about backups and what "never" really means here. Want me to write this up as a short design record?

---

**What to notice:** the coach never proposed base62 IDs or the click log. It asked for options, gave one small hint when you were stuck, and used concrete scenarios (two URLs colliding, a viral post) to let you find the problems yourself.
