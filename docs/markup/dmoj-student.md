# Student Guide — C Programming

A quick reference for using our online judge during contests and practice.

## 1. Logging In

- Use the account created for you at the start of the semester (your student ID as username).
- Forgot your password? The "Forgot your password?" link doesn't work on this system — ask your instructor to reset it for you.

## 2. Joining a Contest

1. Go to **CONTESTS** in the top menu and find today's contest.
2. Click **Join contest**.
3. Once you join, a countdown timer starts (shown at the top or bottom of the page).

**Important:** The timer starts the moment *you personally* click Join — not when the contest officially opens. If the contest window is open from 11:00 to 12:00 and you join at 11:20, your personal timer still starts counting from 11:20.

## 3. "Spectate" Instead of "Join contest"?

If you see a **Spectate** button instead of **Join contest**, it means one of these is true:

- The contest has already ended, **or**
- Your personal time limit has already run out (even if the overall contest window is still open)

In Spectate mode, you can still view the problem and submit code, but **your submissions will not count toward your score**. If you think this happened by mistake, contact your instructor.

## 4. Practicing After a Contest Ends

Once a contest is officially over, its problems usually become available under the **PROBLEMS** tab for unlimited practice — no time limit, and your actual output is shown for failed test cases so you can debug.

If you want to redo a problem **with the original time pressure** (for practice), look for a **Virtual join** option on the contest page. This gives you the same timer as the real contest, but your result is for practice only and does not affect the real scoreboard.

## 5. Writing Your Code — Common Mistakes to Avoid

The judge compares your program's output **exactly** against the expected output. Small mistakes that seem harmless will cause a wrong answer:

- **Don't print prompts before reading input.** Code like `printf("Enter your name: ");` before `scanf(...)` will pollute your output and cause a mismatch — the judge only wants the answer, not a friendly prompt.
- **End every line with `\n` exactly as the problem shows.** If the expected output ends with a newline and yours doesn't (or vice versa), it will not match.
- **Use `==` for comparison, not `=`.** `if (x = 5)` assigns 5 to `x` — it does not compare. This is one of the most common bugs.
- **Declare every variable you use**, and make sure `main` is written in lowercase (`int main(void)`), not `Main`.

## 6. Reading Your Submission Results

After submitting, click on a failed test case to expand it — you'll see **"Your output (clipped)"**, showing exactly what your program printed for that test case. Compare it carefully against what the problem expects.

- **Compilation Error** — your code didn't compile at all. Read the error message; it tells you the exact line and issue.
- **WA (Wrong Answer)** — your code ran, but the output didn't match.
- **TLE (Time Limit Exceeded)** — your code took too long (often an infinite loop).
- **RE (Runtime Error)** — your program crashed while running.

## 7. Getting Help

- Use the **"Report an issue"** button on a problem page if you think the problem statement or test data has an error.
- For login issues, account problems, or anything else, contact your instructor directly.

Good luck, and happy coding!
