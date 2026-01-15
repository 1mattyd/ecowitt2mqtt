#issue.md

Please analyse and fix the GitHib issue: $ARGUMENTS

Follow these steps:

# PLAN
1. Use 'gh issue view' to get the issue details
2. Understand the problem described in the issue
3. Ask clarifying questions if necessary
4. Understand the prior art for this issue
  - Search the scratchpads for previous thoughts related to the issue.
  - Search the PRs to see if you can find history on this issue.
  - Search the codebase for relevant files.
5. Think harder about how to break the issue down into a series of small and manageable tasks.
6. Document your plan in a new scratchpad
  - Include the issue name in the filename.
  - Include the link to the issue in the scratchpad.
7. Let me see your finished plan before you begin creating code.

# CREATE
- Create a new branch for the issue.
- Solve the issue in small, manageable steps, according to your plan.
- Commit your changes after each step.


# TEST
- Write rspec tests to describe the expected behaviour of your code.
- Run the full test suite to ensure you haven't broken anything.
- If the tests are failing, write down each resulting error (with filenames and line-numbers) to a Markdown checklist. Then try fix each item in the checklist.
- Ensure that all tests are passing before moving on to the next step.


# DEPLOY
- Open a PR and request a review.

Remember to use the GitHib CLI ('gh') for all Github-related tasks.



write and run tests to verify the fix
ensure code passes linting and type checking
create a descriptive commit message
