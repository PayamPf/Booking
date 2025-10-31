# Booking
Django Booking System
from pathlib import Path
from reportlab.lib.pagesizes import A4
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, Preformatted
from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
from reportlab.lib.units import mm

# Paths
project_root = Path("/mnt/data/booking_system_enhanced_project")
pdf_path = Path("/mnt/data/booking_system_enhanced_user_code.pdf")

# Collect code files to include
files_to_include = [
    "reservations/models.py",
    "reservations/views.py",
    "templates/dashboard.html",
]

# Prepare PDF
doc = SimpleDocTemplate(str(pdf_path), pagesize=A4,
                        rightMargin=20*mm, leftMargin=20*mm,
                        topMargin=20*mm, bottomMargin=20*mm)
styles = getSampleStyleSheet()
story = []

story.append(Paragraph("Enhanced Booking System - User Provided Code", styles['Heading1']))
story.append(Spacer(1,6))
story.append(Paragraph("This PDF contains the key code files for the enhanced booking system project, showing models, views, and dashboard template.", styles['BodyText']))
story.append(Spacer(1,8))

# Function to add file contents
def add_file_to_pdf(file_path: Path):
    story.append(Paragraph(f"File: {file_path}", styles['Heading2']))
    story.append(Spacer(1,2))
    try:
        code = file_path.read_text(encoding='utf-8')
        story.append(Preformatted(code, ParagraphStyle('Code', fontName='Courier', fontSize=7, leading=9)))
    except Exception as e:
        story.append(Paragraph(f"Error reading file: {e}", styles['BodyText']))
    story.append(Spacer(1,6))

# Add all files
for rel_path in files_to_include:
    add_file_to_pdf(project_root / rel_path)

# Run instructions
run_instructions = """
Running locally (development)
1. Create virtual environment:
   python -m venv venv
   source venv/bin/activate   # Linux / macOS
   venv\\Scripts\\activate    # Windows (PowerShell)

2. Install requirements:
   pip install -r requirements.txt

3. Apply migrations:
   python manage.py migrate

4. Create a superuser:
   python manage.py createsuperuser

5. Run the server:
   python manage.py runserver

6. Access the site in your browser:
   http://127.0.0.1:8000/
   http://127.0.0.1:8000/admin/

Notes:
- Email confirmations will be printed to console.
- Overlapping reservations are prevented.
"""

story.append(Paragraph("Run Instructions", styles['Heading2']))
story.append(Preformatted(run_instructions, ParagraphStyle('Code', fontName='Courier', fontSize=9, leading=12)))

doc.build(story)

print("PDF with user code generated<img width="1024" height="1536" alt="4A66724F-8E42-47F1-BE29-C0D8075F869A" src="https://github.com/user-attachments/assets/1d68a989-cf95-4515-8cf0-2d106015fb26" />
 at:", pdf_path)
