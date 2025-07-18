# Code Scanning Go Tutorial Fork

Welcome to the Code Scanning Go Tutorial! This tutorial will take you through how to set up Github Advanced Security: Code Scanning as well as interpret results that it may find. The following repository contains SQL injection vulnerability for demonstration purpose.

## Introduction

Code scanning is a feature that you use to analyze the code in a GitHub repository to find security vulnerabilities and coding errors. Any problems identified by the analysis are shown in GitHub.

You can use code scanning with CodeQL, a semantic code analysis engine. CodeQL treats code as data, allowing you to find potential vulnerabilities in your code with greater confidence than traditional static analyzers.

This tutorial with use CodeQL Analysis with Code Scanning in order to search for vulnerabilities within your code. 

## Instructions
<details>
<summary>Fork this repo</summary>
<p> 
  
Begin by [forking this repo](https://docs.github.com/en/free-pro-team@latest/github/getting-started-with-github/fork-a-repo).
</p>
</details>

<details>
<summary>Enable Code Scanning</summary>
<p> 

### Security tab

Click on the `Security` tab.


<img src="images/00-repo-security-tab.png" width="80%"/>

### Set up code scanning

Click `Set up code scanning`.

<img src="images/01-repo-secruity-setup-code-scanning.png" width="80%"/>

### Setup Workflow

Click the `Set up this workflow` button by CodeQL Analysis.

<img src="images/02-repo-security-setup-codeql-workflow.png" width="80%"/>

This will create a GitHub Actions Workflow file with CodeQL already set up. Go is a compiled language, so the `autobuild` step is automatically added to your Workflow file. Additional build configurations are documented in [Configuring the CodeQL workflow for compiled languages](https://docs.github.com/en/code-security/code-scanning/automatically-scanning-your-code-for-vulnerabilities-and-errors/configuring-the-codeql-workflow-for-compiled-languages) documentation. 

See the [documentation](https://docs.github.com/en/free-pro-team@latest/github/finding-security-vulnerabilities-and-errors-in-your-code/running-codeql-code-scanning-in-your-ci-system) if you would like to configure CodeQL Analysis with a 3rd party CI system instead of using GitHub Actions.
</p>
</details>

<details>
  
<summary>Actions Workflow file</summary>
<p>

### Actions Workflow

The Actions Workflow file contains a number of different steps including:
1. Checking out the repository
2. Initializing the CodeQL Action
3. Building your project
4. Running the CodeQL Analysis

<img src="images/03-actions-sample-workflow.png" width="80%"/>

Click `Start Commit` -> `Commit new file` to commit the changes to _main_ branch.

<img src="images/03-commit-workflow-file.png" width="80%"/>

</p>
</details>

<details>
  
<summary>Workflow triggers</summary>
<p>

### Workflow triggers

There are a [number of events](https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows) that can trigger a GitHub Actions workflow. In this example, the workflow will be triggered on

- push to _main_ branch
- pull request to merge to _main_ branch
- on schedule, at 4:31 every Saturday

<img src="images/04-actions-sample-events.png" width="80%"/>

Setting up the new CodeQL workflow and committing it to _main_ branch in the step above will trigger the scan.

</p>
</details>


<details>
<summary>GitHub Actions Progress</summary>

<p>
 
### GitHub Actions Progress

Click `Actions` tab -> `CodeQL`

Click the specific workflow run. You can view the progress of the Workflow run until the analysis completes.

<img src="images/05-actions-completed.png" width="80%"/>

</p>
</details>

<details>
<summary>Security Issues</summary>
<p>
  
Once the Workflow has completed, click the `Security` tab -> `Code scanning alerts`. You will see 3 alerts titled "Database query built from user-controlled sources".

<img src="images/06-security-code-scanning-alerts.png" width="80%"/>


### Security Alert View

Clicking on a security alert will provide details about the security alert including: <br/>
<ul>
<li>A description of the issue </li>
<li>A tag to the CWE that it is connected to as well as the type of alert (Error, Warning, Note)</li>
<li>The line of code that triggered the security alert</li>
<li>The ability to dismiss the alert depending on certain conditions (`False positive`? `Won't fix`? `Used in tests`?)</li>
</ul>
<img src="images/06-security-codeql-alert.png" width="80%"/>

### Security Alert Description

Click `Show more` to view a full desciption of the alert including examples and links to additional information.

<img src="images/07-security-codeql-show-more.png" width="80%"/>

### Security Full Description

<img width="80%" src="images/08-security-codeql-full-desc.png">

</p>
</details>

<details>
<summary>Show Paths</summary>
<p>

### Show Paths Button

CodeQL Analysis is able to trace the dataflow path from source to sink and gives you the ability to view the path traversal within the alert.

Click `Show paths` to see the dataflow path that resulted in this alert.

<img src="images/09-security-codeql-show-paths.png" width="80%"/>

### Show Paths View

<img src="images/10-security-codeql-show-paths-details.png" width="80%"/>

</p>
</details>

<details>
<p>  
  
<summary>Fix the Security Alert</summary>

CodeQL has created alerts because our database query is using data supplied by users through the API.  A bad actor could inject a SQL statement to the query parameter which would be executed directly against the database.  To fix this, we need to parameterze our database query.

Click on the `Code` tab and edit the file [`models/models.go`](./models/models.go).  

Within `models.go`, the lines `38`, `57`, and `76` contain the SQL injections.  Remediate those vulnerabilities by following the in-line comments.

<img src="images/11-fix-source-code.png" width="80%"/>

Click `Create a new branch for this commit and start a pull request`, name the branch `fix-sql-injection`, and create the Pull Request.

### Pull Request Status Check

In the Pull Request, you will notice that the CodeQL Analysis has started as a status check. Wait until it completes.

<img src="images/12-fix-pr-in-progress.png" width="80%"/>

### Security Alert Details

After the Workflow has completed click on `Details` by the `Code Scanning Results / CodeQL` status check. 

<img src="images/13-fix-pr-done.png" width="80%"/>

### Fixed Alert

Notice that Code Scanning has detected that this Pull Request will fix the SQL injection vulnerabilities which were detected before.

<img src="images/14-fix-detail.png" width="80%"/>

Merge the Pull Request. After the Pull Request has been merged, another Workflow will kick off to scan the repository for any vulnerabilties. 

### Closed Security Alerts

After the final Workflow has completed, navigate back to the `Security` tab and click `Closed`. Notice that the **Query built from user-controlled sources** security alert now shows up as a closed issue.

<img src="images/15-fixed-alert.png" width="80%"/>

### Traceability

Click on the security alert and notice that it details when the fix was made, by whom, and the specific commit. This provides full traceability to detail when and how a security alert was fixed and exactly what was changed to remediate the issue.

<img src="images/16-fix-history.png" width="80%"/>

</p>
</details>
  
## Next Steps

Ready to talk about advanced security features for GitHub Enterprise? [Contact Sales](https://enterprise.github.com/contact) for more information!

Check out [GitHub's Security feature page](https://github.com/features/security) for more security features embedded into GitHub.

Check out the Code Scanning [documentation](https://docs.github.com/en/free-pro-team@latest/github/finding-security-vulnerabilities-and-errors-in-your-code/about-code-scanning) for additional configuration options and technical details.

