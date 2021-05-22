# changed-files-action
Github Actions for running commands on **only changed files**.

The `action.yml` file in this repo is exampled off **Flake8** for **Python** files.

You can modify this action script for any language and test. To do so, please refer to the instructions below.

## How to use

Refer to text written between the percentile marks `%%%` to modify this action for various languages and tests.

```
name: changed-file-action

on: push

jobs:
  %%% Insert Name Here %%%:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Install Dependencies
      run: |
        %%% Insert Command To Install Module Here %%%
    - name: Get Changed Files
      id: get_file_changes
      uses: dorner/file-changes-action@v1.2.0
      with:
        githubToken: ${{ secrets.GITHUB_TOKEN }}
        plaintext: true
    - name: RESULTS
      run: |
        for directory in ${{ steps.get_file_changes.outputs.files }}
        do
          if [ ${directory: %%% Insert (Extension Length + 1) Here %%%} == "%%% Insert Language Extension Here %%%" ]
          then
            echo Test conducted on $directory
            %%% Insert Module Command Here %%% $directory
          fi
        done
```

For example, running **flake8** on changed **Python** files would look like the following:
```
name: changed-file-action

on: push

jobs:
  Flake8:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Install Dependencies
      run: |
        python -m pip install --upgrade pip
        pip install --upgrade flake8
    - name: Get Changed Files
      id: get_file_changes
      uses: dorner/file-changes-action@v1.2.0
      with:
        githubToken: ${{ secrets.GITHUB_TOKEN }}
        plaintext: true
    - name: RESULTS
      run: |
        for directory in ${{ steps.get_file_changes.outputs.files }}
        do
          if [ ${directory: -3} == ".py" ]
          then
            echo Test conducted on $directory
            flake8 $directory
          fi
        done
```
