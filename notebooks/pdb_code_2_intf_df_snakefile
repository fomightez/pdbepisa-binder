# Save this file as `pdb_code_2_intf_df_snakefile` where you want to run this
# pipeline.
# Snakemake pipeline for generating dataframes from interfaces data for several
# pdb codes. If you just have to make dataframes from interface data for a few 
# structures , then see my notebook 
# `Working with PDBePISA interfacelists in Jupyter Basics.ipynb` that you can 
# run by going to https://github.com/fomightez/pdbepsia-binder
# Needs standard packages for Jupytext and Pandas installed via pip.
# The user has to provide a text file listing the PDB codes with the file named
# `interface_data_2_df_codes.txt` where each line corresponds to the 
# alphanumeric that is the PDB identifier code for a structure.
# 
# To help claify that, the following code between the dashed lines
# can have the starting `#`s removed at the start of each line and that code can 
# be run in a Jupyter notebook to make a list involving PDB entries 6kiv, 6kix, 
# and 6kiz:
#-------------------------
#s='''6kiv
#6kiz
#6kix
#'''
#%store s >interface_data_2_df_codes.txt
#-------------------------
# Behind-the-scenes I, Wayne, largely based this on approaches I developed and 
# used in the snakefile I made for making reports of protein-protein 
# interactions for multiple pairs of related structures
# 
# 
# See the accompanying notebook entitled 
# `Using snakemake to make dataframes displaying the interfaces data for several structures.ipynb`
# for a demo.
# This file can be run after making the text table 
# `interface_data_2_df_codes.txt` by calling
# `!snakemake -j1 -s pdb_code_2_intf_df_snakefile` from inside a jupyter 
# notebook or run via `snakemake` on the command line.
# Via MyBinder, run this Snakefile with the following:
# !snakemake --cores 1 -s pdb_code_2_intf_df_snakefile
# Only 1 core, because I think when I was using 8 it would commonly cause a race
# where more than one notebook was getting auxillary scripts and overwriting as
# as the other notebooks was trying to use. More reliable with 1. But if it, did 
# fail when using more cores, try re-running again because it should just 
# complete the missing files.
# For cleaning, there won't be any conflicts, so use the following on MyBinder:
# !snakemake  -s pdb_code_2_intf_df_snakefile --cores 8 clean
# More general info:
# If you had a ton of comparisons to process and wanted to take advantage of 
# parallel processing in the snakemake run you can read this section:
# Initiate with `snakemake -j X` if the file is named `Snakefile`, 
# replacing the `X` with the number of cores available. Otherwise, initiate with 
# `snakemake -s <snakefile_name> --cores X` if the file is named any thing other
# than `Snakefile`, where `X` is the number of cores.
# To just initiate a rule/step, run something like:
# `snakemake -j 8 <name_of_rules>  -s <snakefile_name>`, where the number 8 is 
# replaced by the result of the command `getconf _NPROCESSORS_ONLN`.

import os
import sys
import datetime
now = datetime.datetime.now()


# FILES THAT WILL BE GENERATED--------------------------------------------------
# initiating_files #initiating files with PDB codes in tname so they look like 
# `pdb_<code>_intf_2_df.txt` to be use to trigger making output files
# pickled_dfs_files # output of pickled dataframe for 
# each PDB code, will look like `<code>_PISAinterface_summary_pickled_df.pkl`
results_archive = (
    f"collection_of_interface_dfs{now.strftime('%b%d%Y%H%M')}.tar.gz") #archive 
    # of #pickled dataframes



##----------------HELPER FUNCTIONS----------------------------------------------
def write_string_to_file(s, fn):
    '''
    Takes a string, `s`, and a name for a file & writes the string to the file.
    '''
    with open(fn, 'w') as output_file:
        output_file.write(s)
##-----------END OF HELPER FUNCTIONS--------------------------------------------




# PREPARATION-------------------------------------------------------------------
# GET THE SCRIPT THAT WILL CONVERT EACH PDB IDENTIFIER CODE INTO A DATAFRAME----
# only get it if it isn't already present in working directory.
file_needed = "pisa_interface_list_to_df.py"
if not os.path.isfile(file_needed):
    os.system("curl -OL https://raw.githubusercontent.com/"\
        "fomightez/structurework/master/"\
        f"pdbepisa-utilities/{file_needed}")


# READ PDB IDENTIFIER CODES LIST & COLLECT PDB CODES & MAKE LIST OF FILES THAT 
# SCRIPT WILL MAKE
# Users provide the information as list of PDB identifier codes with one PDB code
# per line. See above for code that makes an exmaple of that.
# I'm bringing it in here and parsing each line so that I can generate the names
# of the intermediates and output files need to be made. Plus collect the PDB 
# codes as a Python list.
# So that I can use these in snakemake as input and later even as output.
# For now the file with the table must be named `interface_data_2_df_codes.txt`.

file_listing_codes  = "interface_data_2_df_codes.txt" # name of the file with
# PDB identifier codes on each line.
initiating_files = []
pickled_dfs_files = []
list_of_pdb_codes = []
intermediate_to_finals_dict = {} # this dictionary will have the initiating 
# file names as keys with the corresponding resulting output file names as the 
# values
suffix_to_use_for_initiating_files = "_intf_2_df.txt"
suffix_to_use_for_picked_dfs = "_PISAinterface_summary_pickled_df.pkl"
with open(file_listing_codes) as file:
    for line in file:
        pdb_code = line.rstrip()
        initiating_file_to_make = (
            f'pdb_{pdb_code}{suffix_to_use_for_initiating_files}')
        initiating_files.append(initiating_file_to_make )
        pickled_df_file_to_make = f'{pdb_code}{suffix_to_use_for_picked_dfs}'
        pickled_dfs_files.append(pickled_df_file_to_make )
        list_of_pdb_codes.append(pdb_code)
        intermediate_to_finals_dict[initiating_file_to_make] = (
            pickled_df_file_to_make) 


# Use the dict with the initating intermediate file names as keys with the 
# corresponding final file as the value to make the initiating files with the 
# PDB codes in the name that will be used to trigger making a dataframe with the 
# interface data for the PDB code in the file's name.
# I originally had something similar to this part as a snakemake rule with 
# `input: file_listing_codes` & `output: initiating_file`; however, that causes
# an issue because a change to the file 'file_listing_codes' causes snakemake to
# expect all 'initiating_files' should be made to exist and be newer than the 
# updated after the 'file_listing_codes'. And updating that triggers running all
# the next steps. As a result, adding a single PDB code and running snakemake 
# was causing the entire workflow to be run for ALLL PDB codes. Not ideal as
# snakemake workflow where the idea is you can add stuff after the first run & 
# it will only deal with the additional needs.
for initiating_file, picked_df_file in intermediate_to_finals_dict.items():
    pdb_code = initiating_file.split(suffix_to_use_for_initiating_files,1)[0]
    if not os.path.isfile(picked_df_file):
        write_string_to_file(pdb_code,initiating_file)














# SNAKEMAKE RULES---------------------------------------------------------------

rule all:
    input:
        results_archive


# ---------------Individual Rules---------------------------------------------

# Delete any generated files so can trigger full run easily after cleaning
'''
The `touch` commands added make sure files matching each and every pattern of
output so that the `rm` commands don't throw an error.
'''
rule clean:
    shell:
        '''
        touch collection_of_interface_dfs18199xlkleFAKE.tar.gz
        touch pdb_TOTESFAKE_intf_2_df.txt
        touch TOTES18199xlkleFAKE_PISAinterface_summary_pickled_df.pkl
        rm collection_of_interface_dfs*.tar.gz
        rm pdb_*_intf_2_df.txt
        rm *_PISAinterface_summary_pickled_df.pkl
        '''

# Delete any generated files PLUS obtained interface data
'''
Same as 'rule clean' above but adds deleting the `<code>_interface_list.txt`
files that would have been obtained during 'python pisa_interface_list_to_df.py'
step as well as the `pisa_interface_list_to_df.py` script so can easily test 
fresh run during development.
MEANT FOR DEVELOPMENT ONLY.
'''
rule devclean:
    shell:
        '''
        touch collection_of_interface_dfs18199xlkleFAKE.tar.gz
        touch pdb_TOTESFAKE_intf_2_df.txt
        touch TOTES18199xlkleFAKE_PISAinterface_summary_pickled_df.pkl
        rm collection_of_interface_dfs*.tar.gz
        rm pdb_*_intf_2_df.txt
        rm *_PISAinterface_summary_pickled_df.pkl
        touch 18199xlkleFAKE_interface_list.txt
        rm *_interface_list.txt
        touch 18199xlkleFAKEisa_interface_list_to_df.py
        rm *isa_interface_list_to_df.py
        '''










# Make a dataframe for each PDB code
'''
Use the `params` keyword to specify parameter based on the wildcard, which in
this case is the PDB identifier code.
'''
rule run_pisa_interface_list_to_df_for_each:
    input:
        cmd = 'pisa_interface_list_to_df.py',
        code_specifier_file = "pdb_{code}"+suffix_to_use_for_initiating_files
    params: code = "{code}"
    output: "{code}"+suffix_to_use_for_picked_dfs
    shell: 'python {input.cmd} {params.code};rm {input.code_specifier_file}'









# Create archives with the pickled dataframes
'''
I want to have both the pickled files and the archive with a copy of them.
Fortuantely that is what a default tar command does. Including the 
`--remove-files` option would cause it to remove what it archived.
I added some cleaning as well to remove the auxillary script and the 
intermediate text files (`*_interface_list.txt`) used to make the dataframe 
content so that it makes it easier to see pickled dataframe files in the 
directory.

I tried adding in using Rich to say the 'BE SURE TO DOWNLOAD' , see `EVERY_OTHER_RUN_FAILS_AFTER_ADDINGRICH_SO_WENT_BACK_TO_MAKING_ARCHIVE_PLAIN_TEXTmaking snakemake snakefile` 
but if ran and used `devclean` rule a& ran again fairly soon after, usually the 
second one would fail saying it didn't know how to make the archive?!?! Didn't 
understand why and thought it maybe had to do with mixing python in & using 
`shell()` to run the tar command b/c I never had the error about no rule to make 
the archive when the tar command was simpler.
'''
rule make_archive:
    input: pickled_dfs_files
    output: results_archive
    shell:
        '''
        rm pisa_interface_list_to_df.py
        tar -czf {output} {input}; echo "BE SURE TO DOWNLOAD {output}."
        rm *_interface_list.txt
        '''