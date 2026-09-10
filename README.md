# Two-hour workshop for `shepherd-task` skills and scripts

## Caveats

This is the first time I'm giving this workshop. I've tested it on Windows (Dev Box), macOS, and GNU/Linux. There may be additional wrinkles. We'll work through them.

## Run the prepared `simple-math` campaign

1. Clone `https://github.com/edburns/awesome-copilot` and check out the known-good tag of `shepherd-task`.

   I did all the testing with SSH urls.
   
   ```
   git clone git@github.com:edburns/awesome-copilot.git awesome-copilot-01
   cd awesome-copilot-01
   git checkout shepherd-task-v1.0.0
   ```
   
2. Open the `awesome-copilot-01/plugins/shepherd-task/README.md`.

   Ensure the **Local prerequisites** are all satisfied.
  
3. Create a new empty GitHub repository with the following attributes

   - **Owner**: your GitHub ID that is associated with your Microsoft GitHub Copilot license.

   - **Repository name**: Suggested name: `YYYYMMDD-HHMM-simple-math` where `YYYYMMDD-HHMM` are something like `20260909-1943-simple-math` but for your current time and date. The point is uniqueness.

   - **Description**: `shepherd-task simple-math campaign`

   - **Visibility**: Public

   - **Add README**: On

   - **Add .gitignore**: No .gitignore

   - **Add license**: MIT license (my default)

   - Note your fully qualified repository URL. For discussion, `myRepoUrl`.

4. Open the `awesome-copilot-01/plugins/shepherd-task/README.md`.

   Ensure the **Repository prerequisites** are all satisfied.
   
5. Uninstall any previous versions of `shepherd-task` on your system.

   Windows

   ```powershell
   cd .\plugins\shepherd-task\scripts
   .\uninstall-task-shepherd.ps1
   .\install-task-shepherd.ps1
   ```
   
   macOS, GNU/Linux
   
   ```bash
   cd plugins/shepherd-task/scripts/
   ./uninstall-task-shepherd.sh
   ./install-task-shepherd.sh
   ```
   
6. Run the prepared campaign

   Windows

   ```powershell
   cd $HOME\.copilot\plugins\shepherd-task\test\simple-math
   Get-Help .\run-campaign.ps1
   .\run-campaign.ps1 -RepositoryUrl `myRepoUrl`
   ```
   
   macOS, GNU/Linux
   
   ```bash
   cd $HOME/.copilot/plugins/shepherd-task/test/simple-math
   ./run-campaign.ps1 -h
   ./run-campaign.ps1 `myRepoUrl`
   ```
   
## Commentary on the prepared `simple-math` campaign

This section explains what's happening as the `simple-math` campaign runs. It uses sample output from a known-good invocation.

### Windows

`run-campaign` output uses the following semantic palette.

- White: log output from `run-campaign`.

- Dark gray: Planned commands that `run-campaign` will invoke.

- Purple: Actual commands invoked by `run-campaign`.

- Cyan: Stage markers. See `awesome-copilot-01/plugins/shepherd-task/README.md`.

#### Stage 00 - Initialize campaign

```powershell
  & 'C:\Users\edburns\.copilot\plugins\shepherd-task\scripts\shepherd-task-00-init-campaign.ps1' `
      -CampaignIssueNumber '1' `
      -CampaignShortname 'math-control' `
      -BaseBranch 'experiment/shepherd-control' `
      -Repo 'edburns/dd-3056167-01-windows'
```

Invoke the `init-campaign` script. This creates the campaign metadata directory. See section **Campaign** in `awesome-copilot-01/plugins/shepherd-task/README.md`.

Inspect the campaign manifest.

```powershell
cd C:\Users\edburns\workareas\dd-3056162-shepherd-control\2-math-control-remove-before-merge
type .\shepherd-campaign.json
```

See **Campaign manifest** in `awesome-copilot-01/plugins/shepherd-task/README.md`.
   
#### Stage 10 - Create ignorance-reduction plan

```
[shepherd] Normal usage invokes skill shepherd-task-10-create-ignorance-reduction-plan.
[shepherd] Human and Copilot research then fills every implementation-gating Resolution block.
[shepherd] This simple-math fixture substituted an already-resolved math-tool-ignorance-reduction-plan.md for Stage 10 and the research gate.
```

In a real-world invocation of a campaign, here is where you would spend time creating an "ignorance reduction plan". In the prepared sample, a filled in ignorance reduction plan is created for you. Examine the filled in plan, for example 

`C:\Users\edburns\workareas\dd-3056162-shepherd-control\2-math-control-remove-before-merge\math-tool-ignorance-reduction-plan.md`

The section headings of the plan have a required format, as described in `awesome-copilot-01/plugins/shepherd-task/README.md` **Create and resolve the plan when issues do not exist**. 

The ignorance reduction plan should be as detailed as possible. The skill includes several examples of real-world plan and uses them to guide the creation of your plan.

#### Stage 15 - Prepare Stage 20

❌❌There is a known bug in the `run-campaign` for `simple-math` that causes the actual invocation of this stage to appear too late.❌❌

In this stage, the user invokes the script `shepherd-task-15-prepare-create-issues`. This invokes the corresponding skill to read the ignorance reduction plan from the previous step and create issues in the repository issue tracker. A skill is necessary for this because the entire per-issue specification given to the coding agent is included in the issue description.

```powershell
[shepherd] Working directory:
C:\Users\edburns\workareas\dd-3056167-01-windows-shepherd-control
& 'C:\Users\edburns\.copilot\plugins\shepherd-task\scripts\shepherd-task-15-prepare-create-issues.ps1' `
   -CampaignMetadataDirectory '1-math-control-remove-before-merge' `
```

Look at the issues created in your repository. Take this repository as an example.

1. The top level "container" issue. If your repository supports issue types, this would be an **Epic**. https://github.com/edburns/dd-3056167-01-windows/issues/1

2. Individual issues within the container issue for each step in the plan.

   1. https://github.com/edburns/dd-3056167-01-windows/issues/2

   2. https://github.com/edburns/dd-3056167-01-windows/issues/3

#### Stage 25 - Dispatch ordered issue list

```powershell
[shepherd] Working directory:
C:\Users\edburns\workareas\dd-3056167-01-windows-shepherd-control
& 'C:\Users\edburns\.copilot\plugins\shepherd-task\scripts\shepherd-task-25-given-list.ps1' `
    -TaskIssues '2,3' `
    -CampaignMetadataDirectory '1-math-control-remove-before-merge'
[shepherd] Stage 25 will process issues 2,3 serially.
[shepherd] For each issue, Stage 30 moves assignment to the Ready-for-review boundary, then Stage 40 reviews and merges it.
[shepherd] Stage 50 creates the campaign post-mortem after success or failure.
[shepherd] Run evidence will be written beneath: C:\Users\edburns\workareas\dd-3056167-01-windows-shepherd-control\1-math-control-remove-before-merge\shepherd-tasks-<CAMPAIGN_ID>-<TIMESTAMP>
```

See `awesome-copilot-01/plugins/shepherd-task/README.md` **Run stage 25 with an ordered issue list**.

#### Stages 30, 40, 50

By the time you have invoked `shepherd-task-25-given-list` the work proceeds in an entirely human hands-off manner. See `awesome-copilot-01/plugins/shepherd-task/README.md` Sections **Stage 30 readiness boundary** through **Workflow approval helper** and **Post-mortem behavior**.

The post mortem is not pushed by the `shepherd-task` system, you must commit and push it yourself, if desired.

The sample post-mortem is available at https://github.com/edburns/dd-3056167-01-windows/blob/experiment/shepherd-control/1-math-control-remove-before-merge/shepherd-tasks-61e0ba90-97d8-4ee8-b32c-90a2ce3ec2a4-20260909-2010/20260909-2054-post-mortem.md .
