# WordLens — Complete Website Source

This package contains the website's current HTML, CSS, JavaScript and bundled
third-party document readers. It is a static, browser-based application. It has
no custom backend, database, password accounts, or cloud document storage.

## Run locally
1. Extract the ZIP.
2. Install Python 3 if necessary.
3. Open a terminal inside the WordLens_Complete_Website folder.
4. Run:

   python -m http.server 8000 --bind 127.0.0.1 --directory dist

   On Windows you can also use `py` instead of `python`.
5. Open http://localhost:8000 in your browser.
6. Choose Continue as guest to use the analyzer immediately.

Serve over HTTP; opening index.html directly via file:// can prevent PDF workers
and JavaScript modules from loading. No npm install or build is needed.

## Files
- dist/index.html: interface, CSS, live statistics, report export, file upload.
- dist/auth.js: Google profile sign-in flow, guest navigation, sign-out.
- dist/config.js: public Google OAuth client ID configuration.
- dist/documents.js: file-type detection and text extraction.
- dist/vendor/: bundled document parsing libraries, including the PDF worker.
- THIRD_PARTY.md: library versions, sources and license names.

## Activate Google login
Google login is not configured in this source package. Guest mode works.
1. Create/configure an OAuth project and consent screen in Google Cloud Console.
2. Create an OAuth client of type Web application.
3. Add http://localhost:8000 to its Authorized JavaScript origins for local use.
   Add your actual HTTPS website origin when deploying it.
4. Set googleClientId in dist/config.js to your public Google client ID:

   window.WORDLENS_CONFIG = {
     googleClientId: 'YOUR_CLIENT_ID.apps.googleusercontent.com'
   };

5. Save, reload and choose Continue with Google.
6. If the consent screen is in Testing, add the intended Google accounts as test
   users. Popup blockers and Google project restrictions may prevent sign-in.

Alternatively, Google connection settings on the login screen saves a client ID
only in that browser. Editing config.js configures the copy for all visitors.
Do not put a client secret, password or access token in config.js.
The Google flow requests only basic profile/email data, fetches the profile from
Google and holds its token in tab memory. It does not create server accounts or
protect server resources; guest mode intentionally allows local analysis.

## Supported extraction
- PDF with a text layer; DOCX; ODT.
- XLSX, XLS, ODS, CSV and TSV.
- PPTX and ODP.
- PNG, JPEG, WebP and BMP through English OCR.
- UTF-8 text, Markdown, JSON, HTML, XML, YAML, logs and source code.

Limits: 20 MB/file, 2 million extracted characters, PDFs up to 300 pages.
Scanned PDFs without text must first be exported as images for OCR. Old DOC/PPT,
RTF, password-protected documents, archives, executables, audio and video are not
supported. This is not an every-format converter. Review extracted text and OCR
for accuracy. Sentence counts are approximate; reading time assumes 200 wpm.

## Internet requirements and privacy
Document text is processed in your browser, not uploaded by the app.
Most document readers are included locally. Google login requires Google services.
Image OCR downloads its worker, WebAssembly engine and English language model on
first use, so internet access is required. Fonts are loaded from Google Fonts.
No claim of fully offline operation is made.

## Deployment
Upload the contents of dist to any static host that supports JavaScript modules
and worker files. Use HTTPS for Google login and register that origin in Google
Cloud. No project-specific hosting identifiers or repository credentials are
included in this ZIP. The existing hosted site's separate ChatGPT access gate is
a hosting feature and is not part of this standalone application code.

## Verification already completed
PDF, DOCX, XLSX, PPTX and PNG OCR each extracted the test phrase correctly in the
browser. Unsupported binary-file errors and guest navigation were also checked.
Google sign-in cannot be verified end to end until a valid client ID is supplied.
