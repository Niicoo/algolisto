CSV & Excel
===========

CSV
###

`-> CSV Documentation <https://docs.python.org/3/library/csv.html>`_

Formatting Parameters (arguments that can be added to the reader and writers): https://docs.python.org/3/library/csv.html#csv-fmt-params

Below an example to show how to read/write a CSV file:

.. code-block:: python

    import csv

    # Writers
    with open('output1.csv', newline='', mode='w') as csvfile:
        writer = csv.writer(csvfile) # delimiter=',' [default]
        writer.writerow(["Name", "age", "Height", "Weight"])
        writer.writerow(["Gotlib", "88", "174", "75"])
        writer.writerows([
            ["Nicolas", "32", "184", "84"],
            ["Josianne", "67", "185", "82"],
            ["Bernadette", "18", "151", "78"],
        ])

    with open("output2.csv", "w", newline='') as csvfile:
        fieldnames = ["first_name", "last_name"]
        writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
        writer.writeheader()
        writer.writerow({"first_name": "Baked", "last_name": "Beans"})
        writer.writerows([
            {"first_name": "Lovely", "last_name": "Spam"},
            {"first_name": "Bertrand", "last_name": "Haha"},
            {"first_name": "Jean-Luc", "last_name": "Melanchon"},
        ])

    # Reader
    with open("output1.csv", "r", newline='') as csvfile:
        reader = csv.reader(csvfile)
        rows = [row for row in reader]


Excel
#####

The libraries used to read/write excel files are not part of the standard python library. The two main library used are:

- `openpyxl <https://openpyxl.readthedocs.io/en/latest/>`_:  :code:`conda install -c anaconda openpyxl`
- `xlsxwriter <https://xlsxwriter.readthedocs.io/>`_:  :code:`conda install -c conda-forge xlsxwriter`


Below is an example reading/writting excel file using the openpyxl module.

Styling cells (background color, etc...): https://openpyxl.readthedocs.io/en/stable/styles.html#styling-merged-cells


**Writing:**

.. code-block:: python

    from openpyxl import Workbook
    from openpyxl.styles.alignment import Alignment
    from openpyxl.styles import Border, Side, PatternFill, Font, Alignment

    wb = Workbook()

    # Get the first worksheet
    ws1 = wb.active
    ws1.title = "Bisous"
    ws1['A1'] = "Xoxo"

    # Add a worksheet at the beginning (index=0, default=At the end)
    ws2 = wb.create_sheet("Hey", index=0)

    # Different way to access/write to a specific cell
    ca2 = ws2['A2']
    ca2.value = "Ici"
    ws2['b1'] = 4
    cd3 = ws2.cell(row=3, column=4, value=10)
    # Add a formula
    ws2['C3'] = "=Bisous!A1"
    # Styling a cell
    cd2 = ws2.cell(row=2, column=4, value="Style")
    thin_border = Side(border_style="thin", color="000000")
    double_border = Side(border_style="double", color="ff0000")
    cd2.alignment = Alignment("center")
    cd2.border = Border(top=double_border, left=thin_border, right=thin_border, bottom=double_border)
    cd2.fill = PatternFill(fgColor="FFCCCC", fill_type = "solid")
    cd2.font  = Font(b=True, color="FF0000")

    # Saving to a file
    wb.save('output.xlsx')

.. image:: /assets/excel/excel_example.png
   :width: 400pt


**Reading:**

.. code-block:: python

    from openpyxl import load_workbook

    wb = load_workbook('output.xlsx')
    print(wb.sheetnames) # ["Hey", "Bisous"]
    ws = wb["Hey"]
    print(ws['C3']) # "=Bisous!A1"


------------------------------------------------------------

**Sources**:

- CSV Documentation: https://docs.python.org/3/library/csv.html
- openpyxl: https://openpyxl.readthedocs.io/en/latest/
- xlsxwriter: https://xlsxwriter.readthedocs.io/
