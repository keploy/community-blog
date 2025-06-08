---
title: "Introduction to Gitlab Python API"
seoTitle: "GitLab Python API Tutorial for Beginners | Step-by-Step Guide"
seoDescription: "Beginner-friendly GitLab Python API tutorial. Learn setup, authentication, and examples to automate GitLab tasks easily with Python scripts."
datePublished: Sun Jun 08 2025 18:45:44 GMT+0000 (Coordinated Universal Time)
cuid: cmbo0k5qh000302l7d77zhdna
slug: introduction-to-gitlab-python-api
canonical: https://keploy.io/blog/community/introduction-to-gitlab-python-api
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1748660729638/9965e6b9-03a0-49a5-b955-7311050d262e.png
tags: python, gitlab-ci, keploy

---

Doing things manually on GitLab is okay, but it can get repetitive and time-consuming. That's where the GitLab Python API comes to the rescue! With it, you can automate tasks like creating projects, branches, or merge requests using just a few lines of Python code. It's incredibly handy for DevOps, scripting, or personal projects!

In this blog, we'll dive into setting up and using the GitLab Python API step by step. You'll discover how to connect it with your GitLab account and perform amazing actions with code. We'll even explore how to run Keploy tests in GitLab CI. It's an awesome way to make your GitLab workflow easier and more fun with Python!

## Why Do We Need the GitLab Python API?

While GitLab offers a clean and user-friendly web interface, it becomes inefficient when you need to repeat the same tasks across multiple projects. Manually creating repositories, branches, and merge requests is fine once or twice, but quickly turns tedious at scale.

The GitLab Python API helps automate these repetitive tasks. With just one script, you can create and configure entire repositories, push code, create branches, and even open merge requests all in seconds. This automation reduces errors and saves valuable time.

By using the API, developers can streamline their workflow, especially in large teams or CI/CD environments. It empowers you to handle bulk operations programmatically, freeing you from the limitations of manual UI interactions.

Want to go further with Python APIs? Here’s how to [*build and deploy REST APIs*](https://keploy.io/blog/community/introduction-to-rest-api-in-python) in Python using Flask – a great foundation for what we’re doing here.

## How to Set Up the GitLab Python API?

The GitLab Python API lets you automate all of that with a few lines of code. Here’s a simple 4-step guide to get started:

### Prerequisites

Before we jump into the setup, make sure you have the following ready:

* **Python** (version 3.6+ is preferred)
    
* **Flask(** version **2.3+ is compatible)**
    
* **pip** (Python package manager, usually comes with Python)
    
* **VS Code(recommended)** or any code editor
    
* A **GitLab account**
    

> To check if Python and pip are installed, run:

```bash
python --version
pip --version
pip install Flask
```

If you don’t have Python, download and install it from the website. During installation, make sure to check the box that says **“Add Python to PATH.”**

## How to Install the GitLab Python Library?

Once Python is ready, open your terminal or command prompt and install the GitLab package

```bash
pip install python-gitlab
```

This will install the official GitLab Python client, which lets your Python code talk to GitLab’s REST API behind the scenes.

> 🔍 To make sure it’s installed:

![Verify python-gitlab installation in the terminal](https://cdn.hashnode.com/res/hashnode/image/upload/v1748435652754/ad2c6fce-dfc0-4f2d-a9ac-94cb3a6795ef.png align="center")

You should see some metadata printed if the installation went through properly.

## How to Generate a Personal Access Token in GitLab?

To let your Python script access GitLab on your behalf, you'll need to generate a token, think of it as an API Key or a password.

Here’s how to generate it:

1. Go to GitLab and log in.
    
2. Click on your profile picture → **Edit profile**.
    
3. From the sidebar, go to **Access Tokens**.
    
4. Give your token a **name** (like `automation-token`).
    
5. Set an **expiration date** (optional, but recommended).
    
6. Under **Scopes**, check:
    
    * ![Allowing access for api,read_api,read_user by checking the boxes in settings of the repository](https://cdn.hashnode.com/res/hashnode/image/upload/v1748436448540/898554aa-6dbd-46c3-8e0d-b559ab8f7e13.png align="left")
        
        `api` (for full access)
        
    * `read_api` (to read repo content)
        
    * `read_user` (to push changes)
        
7. Click **Create personal access token**.
    
8. **Copy the token immediately,** as you won’t see it again later.
    

Discover how to integrate Keploy with GitLab CI/CD pipelines to automate testing processes, ensuring tests run with every commit and merge request.

## How to Connect to GitLab Using the Python API?

To use GitLab API in your code, you need to import it like this at the top of the file. Create a sample main.py file where we’ll play around with the code.

```python
#step 1
import gitlab
# step 2
gl = gitlab.Gitlab(
    url='https://gitlab.com', 
    private_token='your_personal_access_token_here'
)
gl.auth()
```

Once you have the token, you can connect to GitLab with just a few lines of Python:

Paste your access token here. If there’s no error, you’re all set! That’s it, now you can start performing actions like creating projects, branches, and even handling merge requests all through code.

## How to perform operations on GitLab using the Python API?

So far, we’ve created a main.py file where we imported the `gitlab` package and connected to the GitLab API using a personal access token (PAT). This simple step of authentication allows us to interact with our GitLab account programmatically, just like a user, but through code!

Once authenticated, you can automate pretty much everything you'd normally do through the GitLab web interface. That includes creating a project, pushing files, making commits, handling branches, and even merging changes. With just a few lines of Python code, you can set up an entire workflow.

Here’s what we’re going to cover, step-by-step, as in the flowchart:

![Complete Flowchart on using gitlab python API and performing operations on a repository](https://cdn.hashnode.com/res/hashnode/image/upload/v1748529698454/b83f501e-0b40-4ed5-8b6c-23c347dbe85e.png align="left")

We’ll go through each one in a very simple, straightforward way with relevant Python code snippets and explain what's going on. This will help you automate your GitLab tasks and speed up your development cycle, especially when working with multiple projects or teams.

## How to create a new project on GitLab using the Python API?

Firstly, we are creating a project and assigning it a name. In this case, I would like to give it hello-world project to simplify the process. As you can see, the GitLab package with the projects variable creates a new project with the assigned project name using the create method, and once created, we print the project name. That’s it, we have created our project.

```python
#step 1
import gitlab
# step 2
gl = gitlab.Gitlab(
    url='https://gitlab.com', 
    private_token='your_personal_access_token_here'
)
gl.auth()

project_name = 'hello-world-js'
project = gl.projects.create({'name': project_name})
print(f"Created project: {project.web_url}")
```

## How to create a new branch on GitLab using the Python API?

To this, we can create a branch on GitLab. Branch is nothing but it allows us to make changes safely without disturbing the original code branch. It’s more like a copy of your codebase. Here, we’ve created a feature-hello branch.

```python
#step 1
project.branches.create({'branch': 'feature-hello', 'ref': 'main'})
print("Created new branch: feature-hello")

# 1. Get some project info
def get_project_info():
    print(f"Name: {project.name}")
    print(f"Description: {project.description}")
    print(f"Default branch: {project.default_branch}")
    print(f"SSH URL: {project.ssh_url_to_repo}")
    print(f"HTTP URL: {project.http_url_to_repo}")
```

To run this file, open your terminal, navigate to the folder and run the command python followed by the file name.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1748660944864/38d855b0-a892-4eb9-8a5a-00f94e90dfac.png align="center")

That’s it. We have successfully created a project with a branch. Now, let’s get some info about this newly created project via the API.

As you can see, we have defined a get\_project\_info function where we’re printing the project name, description, default branch and so on. When you execute it, you get the following output.

![Output for get_project_info() showing the repo name,description,default branch,SSH and HTTP URLs.](https://cdn.hashnode.com/res/hashnode/image/upload/v1748449799563/85c2162c-9ac1-4a59-83e5-7fdf0e02d886.png align="center")

## How to Add Files to the Repository on GitLab Using the Python API?

Now let’s add some files to the existing repository. For the hello-world application, I’ve hardcoded a sample Python file that just prints Hello world!, and it is saved in the flask\_content variable.

```python
#step 1
import gitlab
# step 2
gl = gitlab.Gitlab(
    url='https://gitlab.com', 
    private_token='your_personal_access_token_here'
)
gl.auth()

project_name = 'hello-world'
project = gl.projects.create({'name': project_name})
print(f"Created project: {project.web_url}")

flask_content = """
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "<h1>Hello from Python Flask!</h1>"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)

"""

project.files.create({
    'file_path': 'main.py',
    'branch': 'feature-hello',
    'content': flask_content,
    'commit_message': 'Add Hello World Flask app'
})
print(" Added index.html to feature-hello branch")
```

And then, using the create method option with the project variable, include the file path and name it as per the convention, mentioning the branch name and the content you want to write in the file with a commit message to store your history of the project.

After doing so, it prints a message saying “Added main.py to the branch”.

## How to Merge Branches on GitLab Using the Python API?

Merging keeps track of that merge with a new commit. It makes a new commit when you merge something. If you cross-check it on GitLab, you can find two branches: one is main and another is feature-hello.

Ensure you’re on the main branch whenever you’re merging. You can also do this via the terminal by listing current branches.

```python
# 1. List all branches
def list_branches():
    branches = project.branches.list(all=True)
    print("Branches:")
    for branch in branches:
        print(f"- {branch.name}")

# create merge requests
mr = project.mergerequests.create({
    'source_branch': 'feature-hello',
    'target_branch': 'main',
    'title': 'Merge Hello World App'
})

# print some details regarding merge request
print(f"Merge Request Title: {mr.title}")
print(f"MR State: {mr.state}")
print(f"MR Merge Status: {mr.merge_status}")
print("Merge successful.")
```

To merge branches using the GitLab Python API, start by creating a merge request with the `project.mergerequests.create()` method. Provide the source branch, target branch, and a title for the request. For example, to merge `feature-hello` into `main` , set those as the respective branches.

Once the merge request is created, check its status. If the state is `opened` and the merge status is `can_be_merged`, it means the merge can proceed without conflicts. You can then trigger the merge using the `merge()` method.

![Output listing the branch names through list_branches()](https://cdn.hashnode.com/res/hashnode/image/upload/v1748449036935/617c1ddd-d3ee-46e7-b3b8-6de21697b2dc.png align="left")

![Output showing the Merge request,state status and successful message for a merge commit.](https://cdn.hashnode.com/res/hashnode/image/upload/v1748437254042/d1f7dd67-238f-4a01-9052-9ff98ccae293.png align="left")

On success, the script prints a confirmation like "Merge successful," letting you know the branches have been merged cleanly, all handled within Python.

![Screenshot of a terminal showing a Python script execution. A merge request is created for a GitLab repository, but merging is not possible at the moment.](https://cdn.hashnode.com/res/hashnode/image/upload/v1748450820235/1aa22d2c-8519-4f83-b8ca-ce62beda32d0.png align="center")

### Performing CRUD operations

You can also perform update, create operations on the file and then delete the file by writing simple Python functions like below.

```python
#step 1
import gitlab
# step 2
gl = gitlab.Gitlab(
    url='https://gitlab.com', 
    private_token='your_personal_access_token_here'
)
gl.auth()

project_name = 'hello-world'
project = gl.projects.create({'name': project_name})
print(f"Created project: {project.web_url}")

# 2. Create file in a branch
def create_file(branch, file_path, content, commit_message):
    try:
        project.files.create({
            'file_path': file_path,
            'branch': branch,
            'content': content,
            'commit_message': commit_message
        })
        print(f"File '{file_path}' created in branch '{branch}'.")
    except gitlab.exceptions.GitlabCreateError as e:
        print(f"Error creating file: {e}")

def update_file(branch, file_path, new_content, commit_message):
    try:
        f = project.files.get(file_path=file_path, ref=branch)
        f.content = new_content
        f.save(branch=branch, commit_message=commit_message)
        print(f"File '{file_path}' updated in branch '{branch}'.")
    except Exception as e:
        print(f"Error updating file: {e}")

# 4. Delete file in a branch
def delete_file(branch, file_path, commit_message):
    try:
        f = project.files.get(file_path=file_path, ref=branch)
        f.delete(branch=branch, commit_message=commit_message)
        print(f"File '{file_path}' deleted from branch '{branch}'.")
    except Exception as e:
        print(f"Error deleting file: {e}")
```

Running these on the terminal gives the following output.

![Text indicating that "test.txt" was created and updated in branch "main".](https://cdn.hashnode.com/res/hashnode/image/upload/v1748450659186/50400f31-0cc9-49d1-826b-c5ae64a99867.png align="left")

![Screenshot of code text indicating "File 'test.txt' deleted from branch 'main'."](https://cdn.hashnode.com/res/hashnode/image/upload/v1748450704496/f6c37be8-1c96-496f-9247-0f93f9afbfd1.png align="left")

## Automate keploy tests with ease in Gitlab CI

Keploy lets you automatically test your Flask app by recording real HTTP traffic and converting it into test cases. You can then replay them in GitLab CI pipelines with every push or merge.

But wait, are you hearing about Keploy for the first time? Then give it a read [here](https://keploy.io/blog/community/getting-started-with-keploy) about how Keploy helps in performing automated testcases.

### 1\. Add `.gitlab-ci.yml` to Your Project Root

This YAML file tells GitLab what to do during each CI pipeline stage. Here’s a version for your Python (Flask) app:

```yaml
image: python:3.10

stages:
  - test
  - deploy

before_script:
  - pip install flask

test_app:
  stage: test
  script:
    - python -m unittest discover

deploy_app:
  stage: deploy
  script:
    - python app.py
```

* Uses the Python image.
    
* Installs Keploy binary.
    
* Installs your app dependencies.
    
* Runs the app through Keploy to **record tests**.
    

### **2\. Record Keploy Tests Locally for a Flask App**

To generate `.keploy/tests` for your Flask app, use the `keploy record` command by specifying your Python server script.

Check out how Keploy records test cases from Python APIs in this [Python-Flask tutorial](https://keploy.io/blog/testing-python-apps-using-keploy/).

#### Step-by-step:

1. **Make sure Keploy is installed and your Flask app is ready.**  
    If your main app file is `main.py`, and it runs your server, you’re good to go.
    
2. **Run this command in your terminal:**
    
    ```python
    sudo -E keploy record --c "python app.py"
    ```
    
3. **Interact with your app**  
    Once the server starts, open a browser or use tools like `curl` or Postman to make requests to your Flask routes.  
    Example:
    
    ```python
    curl http://localhost:5000/
    ```
    
4. **Keploy will capture these requests** and store them as test cases inside the `.keploy/tests` folder for replay or regression testing.
    

### 3\. Push to GitLab

Commit and push your `.gitlab-ci.yml` source code. GitLab will then:

* Run your Flask app, and simultaneously, Keploy will start recording tests. You’ll see the logs in the CI job output as shown below.
    
    You can visit the app at [http://127.0.0.1:5000](http://127.0.0.1:5000) and quit using Ctrl + C.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749007960883/84076daa-243b-417d-affd-3f3e796092c8.png align="left")
    
    Once you run the above command, you will see that Keploy is initialized and probes are added to the kernel. Automatic testing begins, capturing test cases and storing each one in a YAML file. These files are then saved in the .keploy/tests folder once the application starts running.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749007996129/a1fb5fad-e2f3-4d02-b696-782912d62a24.jpeg align="left")
    

Initialize the project using Git, then commit and push the changes to GitLab. After pushing, you can see that Keploy automatically triggers the pipeline on the repository page under Build &gt; Pipelines &gt; View Pipeline Analytics.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749020029919/94e22ccb-7b4c-4213-9fee-35e836f4bcbc.png align="center")

Once finished, you will see that the CI pipeline has been triggered, and it records all test cases in the `.keploy/tests` folder in the GitLab web application. It's that simple, trust me!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749020796436/debcbf97-dea8-41da-b14f-c7957c93c9fb.png align="center")

Congrats😄, It’s some Keploy magic🪄

## Conclusion

In this blog, we’ve explored how to use the GitLab Python API to automate key tasks like project creation, branching, file management, and merging. We also learned how to interact with repositories and streamline workflows directly from Python. These capabilities help reduce manual effort and boost developer efficiency.

We then integrated Keploy tests into GitLab CI to bring reliability and observability into our DevOps pipeline. This empowers teams to test smarter and deliver better software faster. With these tools in hand, we’re well-equipped to build, test, and ship with confidence. Let’s keep exploring and automating!

## Related Blogs

1. **CI/CD** - 🔗[Keploy/docs/CI-CD](https://keploy.io/docs/ci-cd/gitlab/)
    
2. **End-to-End testing** - 🔗[integration-of-e2e-testing-in-a-cicd-pipeline](https://keploy.io/blog/technology/integration-of-e2e-testing-in-a-cicd-pipeline)
    
3. **Top CI tools** - 🔗[top-ci-tools-for-efficient-software-development](https://keploy.io/blog/community/top-ci-tools-for-efficient-software-development)
    

## FAQs

**Q1. How do I get my GitLab private token?**  
**A**: Go to GitLab → User Settings → Access Tokens → Generate a new token with required scopes like `api` and `read_repository`.

**Q2. Can I manage multiple GitLab projects using the Python API?**  
**A**: Yes, you can fetch and manage any number of projects by referencing them via their namespace and project name.

**Q3. Is it safe to store my private token in the script?**  
**A**: It’s better to store your token in environment variables or a `.env` file to keep it secure.

**Q4. Can I use the GitLab API to automate CI/CD workflows?**  
**A**: Absolutely! You can trigger pipelines, check pipeline status, and integrate tools like Keploy using the API.

**Q5. What if I accidentally overwrite or delete something using the API?**  
**A**: Always test on dummy projects first. The API gives full access, so mistakes can directly affect your repository.