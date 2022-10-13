# Python save code for reproduce
A simple code to save needed python code for running in order to keep the reproducibility of experiment.
To use this code you should give the file and folder relative path and the path you want to save.
```python
# example
save_code(["./main.py", "option.py", "model", "run_script"], "./result/expriment1/code")
```

```python
# usage
save_code(["./main.py", "option.py", "model", "run_script"], "./result/expriment1/code")

# function definition
def save_code(file_path_list, code_save_dir):
    """
    file_list: ["file_1.py", "folder1", ...]
        NOTE: 1. please give relative path
              2. for folder only support automate search .sh and .py files to save

    code_save_dir: the destination to save the code.
    """

    # create the save directory
    os.makedirs(code_save_dir, exist_ok=True)

    for path in file_path_list:
        
        if os.path.isdir(path):  
            # for bash file
            # search files and save it 
            pathname = os.path.join(path, "/**/*.sh")
            files = glob.glob(pathname, recursive=True)
            for file in files:
                dest_fpath = os.path.join(code_save_dir, file)
                try:
                    shutil.copy(file, dest_fpath)
                except IOError as io_err:
                    os.makedirs(os.path.dirname(dest_fpath))
                    shutil.copy(file, dest_fpath)

            # for python file
            # search files and save it 
            pathname = os.path.join(path, "/**/*.py")
            files = glob.glob(pathname, recursive=True)
            for file in files:
                dest_fpath = os.path.join(code_save_dir, file)
                try:
                    shutil.copy(file, dest_fpath)
                except IOError as io_err:
                    os.makedirs(os.path.dirname(dest_fpath))
                    shutil.copy(file, dest_fpath)

        elif os.path.isfile(path):  
            dest_fpath = os.path.join(code_save_dir, path)
            os.makedirs(os.path.dirname(dest_fpath), exist_ok=True)

            shutil.copy(path, os.path.join(code_save_dir, dest_fpath))
        else:  
            print("{} is a special file (socket, FIFO, device file). Not support.".format(path) )
```

# A simple code to save and load argument
```python
from argparse import ArgumentParser
import json

# definite argument parser
parser = ArgumentParser()
parser.add_argument('--seed', type=int, default=8)
parser.add_argument('--resume', type=str, default='a/b/c.ckpt')
parser.add_argument('--surgery', type=str, default='190', choices=['190', '417'])
args = parser.parse_args()

# save args to a txt file
with open('commandline_args.txt', 'w') as f:
    json.dump(args.__dict__, f, indent=2)

parser = ArgumentParser()
args = parser.parse_args()
# load args from a json file
with open('commandline_args.txt', 'r') as f:
    args.__dict__ = json.load(f)

print(args)
```

# To generate a time stamp
```python
from datetime import datetime
time_stamp = datetime.now().strftime("%Y%m%d%H%M%S")
```


