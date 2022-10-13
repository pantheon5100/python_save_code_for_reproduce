# python_save_code_for_reproduce
a simple code to save needed python code in order to keep the reproducibility of experiment

"""python

def save_code(file_path_list, code_save_dir):
    """
    file_list: [file_1.py, folder1, ...]
        NOTE: 1. please give relative path
              2. for folder only support .sh and .py

    code_save_dir: the destination to save the code.

    """

    # create the save directory
    os.makedirs(code_save_dir, exist_ok=True)

    for path in file_path_list:
        
        if os.path.isdir(path):  
            # for bash file
            # search files and save it 
            pathname = path + "/**/*.sh"
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
            pathname = path + "/**/*.py"
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
            print("It is a special file (socket, FIFO, device file)" )

"""
