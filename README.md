# Labelling Japanese CVs with PAWLS

This documents describes the installation of PAWLS, an annotation tool for PDFs, and the set of standards for labelling Japanese CVs with it. For the original README, please go to [allenai/pawls](https://github.com/allenai/pawls).


## Running PAWLS

### From a remote machine
Simply open `http://192.168.2.54:8080` on your browser. The IP you see is currently Samuel's PC.

### Installing it locally
Please first have the latest version of [🐳Docker](https://www.docker.com/get-started/) installed on your local machine.
1. Launch Docker. 
2. Navigate to the directory you want PAWLS to be in.
3. If you're on Windows, run `git config --global core.autocrlf false` in your terminal to fix end-of-line issues.
4. `git clone https://github.com/slo-daxtra/pawls.git`
5. Download the dataset from `https://drive.google.com/drive/folders/1m-c8yhl_Pl7xDDxlaomK2AwojXvtykfF`, and put the contents in `pawls/skiff_files/apps/pawls/papers`, so for example an example folder is located in `pawls/skiff_files/apps/pawls/papers/34889_CV.pages_1_2`.
6. `cd pawls`
7. `docker-compose up --build`


## Labels

Only label the content, and not the heading. For example, for education history, only label the content but not the heading "学歴". If it's impossible (i.e. if they are the same box as the content), then please enable "Free Form Annotations" from the labels section, and please remember note down what labels were free-form, so we could fix the boxes manually afterwards.

* `phonetic_name`: Name in furigana
* `family_name`: Family name (mostly in kanji)
* `given_name`: Given name (mostly in kanji)
* `place_of_birth`: Town or prefecture of origin
* `date_of_birth`: Date of birth excluding "生"
* `age`: Age only including the number (avoid "満" or "歳" if possible)
* `gender`: Self-explanatory
* `current_date`: Date of last update for the CV (avoid "現在" if possible)
* `tel_full`: If area code and main number are on the same line, use this
* `tel_area_code`: Area code, if it's on a different line from the main number
* `tel_body`: Main number excluding the area code, if it's on a different line from the area code
* `tel_type`: What kind of phone number it is (e.g. "電話", "携帯電話", etc.)
* `email`: Email address
* `postcode`: Post code part of the address (avoid "〒" if possible)
* `address_line`: Main address, excluding the postcode
* `edu_start`: Full start date of an education history
* `edu_end`: Full end date of the education history (if available)
* `edu_desc`: Description of the education history
* `work_start`: Full start date of a work history
* `work_end`:  Full end date of the work history (if available; and exclude things such as "同行を退社")
* `work_desc`: Description of the work history
* `qualif_start`: Full start date of a qualification history
* `qualif_end`: Full end date of the qualification history (if available)
* `qualif_descrtipion`: Description of the qualification history
* `skill`: Skill item, including language


## Data structure

1. The files ready to be labelled should be in `pawls/skiff_files/pawls/papers`.
2. Each PDF is a folder, and inside you'll see the original PDF and a file called `pdf_structure.json`, which has information about the PDF and the pre-made boxes you'll be labelling.
3. Once you make an edit on a certain PDF, a file called 


## Start labelling

Once you have PAWLS open, select the file you want to label on the left, below the labels.

### Annotations

1. To make an annotation, select the label you want (either with your mouse, number keys, or left and right arrow keys).
2. Drag on the screen the region you want to label. You don't need to be precise since it'll automatically snap into place to surround the selected text.
3. Repat the above.

### Relations

We need to label relations when two or more elements are related, such as `work_start`, `work_end` and `work_desc` related to the same job. 

1. To label and relation, make sure you already have all the annotations.
2. Hold down shift, and click all the relevant labels (e.g. the `work_start`, `work_end` and `work_desc` labels for the same job).
3. When you release the shift key, you'll see a window called "Annotate Relations".
4. There is only one relation label available for now ("Related").
5. Put the dates on the left, and the descriptions on the right.

Try not to make mistakes when labelling relations, because they're harder to remove. If you have to remove a relation, you can do so by directly modifying `slo-daxtra_annotations.json` inside the correct folder in `pawls/skiff_files/pawls/papers`.

## Output files
The output is a file called with `slo-daxtra_annotations.json` inside each PDF file's folder. 
