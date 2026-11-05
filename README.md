# WhisprNet

WhisprNet is a college project prototype for role-based file sharing with an approval workflow. The Django app has three roles: administrator, distributor, and user. Users can submit files to their distributor; distributors can approve or reject access requests; approved users can browse shared records and download files. The project also includes an email OTP step and an experimental file-encryption flow.

The report gives the project the academic title “An Anonymity Enhanced Cloud Collaboration (AEDC) Framework for Secure Group Data Sharing in Cloud Environments.” That title describes the intended design. The current code does not implement cryptographic group signatures or attribute-based key agreement: its “signature” is a randomly generated six-digit value stored with the user record and requested as a second login step. The application also retains names, email addresses, and other profile details. Treat WhisprNet as a role-based file-sharing prototype, not as an anonymity system.

## Technology

- Python and Django
- MySQL through PyMySQL for the application's project tables
- SQLite for Django's built-in admin/session tables by default
- HTML, CSS, JavaScript, and static template assets
- cryptography for the prototype encryption functions

## Project layout

- pro/ contains Django project settings and URL configuration.
- p_app/ contains the application views, routes, templates, and static assets.
- b_app/ contains the upload destination used for registration profile images.
- database/schema.sql creates an empty MySQL schema for the application.

Uploaded files, profile images, local databases, and the original data dump are intentionally excluded from version control because the working copy contains private records and uploaded content.

## Local setup

1. Install Python 3.10 and MySQL, then create a virtual environment and install dependencies:

   ```bash
   python -m venv .venv
   # Windows: .venv\Scripts\activate
   # macOS/Linux: source .venv/bin/activate
   pip install -r requirements.txt
   ```

2. Create a MySQL database using `database/schema.sql`.
3. Copy `.env.example` to `.env` and set the database and Django values. The application does not load `.env` automatically; export those variables in your shell or load them with your preferred environment-variable tool.
4. Run Django's built-in database setup and start the development server:

   ```bash
   python manage.py migrate
   python manage.py runserver
   ```

The application code uses the MySQL tables directly. The schema file creates empty tables; there are no sample accounts or credentials in this repository.

## Security and project status

This is an academic prototype and has not been assessed for production use. The current application contains legacy SQL assembled from request values, prototype password handling, limited authorization checks, and an AES-CBC design with a fixed IV. Do not use it with real user data or deploy it to the public internet without a security redesign. Secret values are read from environment variables and should never be committed.

The report marks 37 test cases as passing. The supplied code does not include an automated test suite or test logs that reproduce those results; `p_app/tests.py` is only a placeholder. Key rotation is performed in the approval path and processes files synchronously. Rejection does not rotate the group key. File sharing records a recipient and sender, but cross-group decryption is not demonstrated by the supplied code. Verify these behaviors before describing them as working features.

The project report contains identifiable information in screenshots, so it is not included in this public repository. The local database, SQL data dump, and user-uploaded files are excluded as well.

## License

The included third-party template license is in `p_app/static/LICENSE.txt`. No separate license for the WhisprNet project source was identified in the supplied working copy.
