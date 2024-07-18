# check_xml_file
XML Payroll Validator and Exporter

Description
This project reads XML files containing payroll data, extracts and processes the information, and generates detailed Excel (XLSX) files with all the records as well as summary data. It is designed to ensure the accuracy of the XML files before they are sent for payment. The project runs on Google Colab.

Features
XML Parsing: Reads and parses XML payroll files.
Data Extraction: Extracts detailed payroll information including gross amounts, deductions, employer taxes, and net amounts.
Excel Export: Creates Excel files with detailed records and summary data.
Validation: Helps verify the accuracy of the XML payroll files before processing payments.
Installation
To run this project, you'll need to install the following Python packages in Google Colab:


import xml.etree.ElementTree as ET
import openpyxl
from openpyxl.styles import Font
from google.colab import files, drive
import time
Usage
Setup and Initialization
Clone the repository:


git clone https://github.com/yourusername/your-repo-name.git
cd your-repo-name
Open the script in Google Colab:

Upload the script to Google Colab or copy the content into a new Colab notebook.

Mount Google Drive:


drive.mount('/content/drive', force_remount=True)
Upload and Process XML Files
Upload XML Files:

Run the following function to upload XML files from your local machine to Google Colab:

def upload_xml_files():
    uploaded = files.upload()
    file_paths = []
    for file_name, file_content in uploaded.items():
        file_path = f"/content/{file_name}"
        with open(file_path, "wb") as f:
            f.write(file_content)
        file_paths.append(file_path)
    return file_paths

print("Please upload XML files:")
xml_files = upload_xml_files()
Extract Data from XML:

Extract detailed payroll information from the uploaded XML files:


def extract_data_from_xml(file_path):
    # Implementation of XML data extraction
    ...
Write Data to Excel:

Create Excel sheets with detailed records and summary data:


def write_data_to_sheet(sheet, data):
    # Implementation of writing data to Excel sheet
    ...

def write_sums_to_sheet(sheet, sums, gross_total, deduction_total, employer_tax_total, netAmount1_total, unique_employees):
    # Implementation of writing sums and summary to Excel sheet
    ...
Calculate Sums and Summary:

Aggregate and summarize the extracted data:


def calculate_sums(data):
    # Implementation of calculating sums
    ...

def write_final_summary(sheet, all_sums, all_gross, all_deduction, all_employer_tax, all_netAmount1):
    # Implementation of writing final summary to Excel sheet
    ...
Process and Export Data:

Process each XML file, aggregate data, and save the final Excel file:


output_path = '/content/drive/MyDrive/OLKES/files/xml_sums_str.xlsx'
start_time = time.time()

if xml_files:
    workbook = openpyxl.Workbook()
    default_sheet = workbook.active
    workbook.remove(default_sheet)

    all_sums = {}
    all_gross = []
    all_deduction = []
    all_employer_tax = []
    all_netAmount1 = []

    for idx, xml_file in enumerate(xml_files):
        data, gross_total, deduction_total, employer_tax_total, netAmount1_total, unique_employees = extract_data_from_xml(xml_file)
        data_sheet = workbook.create_sheet(f"Data_{idx + 1}")
        write_data_to_sheet(data_sheet, data)
        sums = calculate_sums(data)
        sums_sheet = workbook.create_sheet(f"Sums_{idx + 1}")
        write_sums_to_sheet(sums_sheet, sums, gross_total, deduction_total, employer_tax_total, netAmount1_total, unique_employees)

        for key, value in sums.items():
            if key not in all_sums:
                all_sums[key] = 0
            all_sums[key] += value
        all_gross.append(gross_total)
        all_deduction.append(deduction_total)
        all_employer_tax.append(employer_tax_total)
        all_netAmount1.append(netAmount1_total)

    summary_sheet = workbook.create_sheet("Summary")
    write_final_summary(summary_sheet, all_sums, all_gross, all_deduction, all_employer_tax, all_netAmount1)
    workbook.save(output_path)
    print(f"Workbook saved to {output_path}")
else:
    print("No XML files uploaded. Please upload at least one XML file.")

end_time = time.time()
print(f"Time taken: {end_time - start_time:.2f} seconds")
Contributing
Contributions are welcome! Please fork the repository and use a feature branch. Pull requests are reviewed on a regular basis.

License
This project is licensed under the MIT License - see the LICENSE file for details.

Contact
If you have any questions or need further assistance, feel free to open an issue in the repository.
