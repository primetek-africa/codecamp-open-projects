# FarmPlot Lease Match Architecture

## Shape
A public listing client and private owner workspace use a single API and relational database. Add a moderation area for listing review.

## Main modules and records
- User profiles with owner, farmer, or agent roles.
- Plot listing: approximate area, season, land features, terms, status.
- Enquiries with participant references and status.
- Lease checklist and optional agreement document reference.

## Main flow
An owner creates a listing with approximate location. A farmer searches and sends an enquiry. Both parties discuss terms and update the status; any agreement is made outside the platform.

## Operations and safeguards
Do not expose exact plot coordinates before an owner chooses to share them. Clearly distinguish user claims from verified details and avoid presenting listings as legal title evidence.
