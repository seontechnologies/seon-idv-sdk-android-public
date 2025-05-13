# SEON ID Verification Android SDK Error Codes

This section describes the possible error codes returned by the SEON Android SDK, along with suggested checks and resolutions for each case.

## ERROR_CODE_1

### Description:

The current session is unrecoverable.

### Resolution:

-   Start a new session by reinitializing the SDK with a fresh configuration.
-   Check if you have provided correct baseUrl, customerData and licenseKey (associated with correct application ID).


## ERROR_CODE_2

### Description:

Failure in fetching session data or initializing the SDK due to missing or incorrect parameters.

### Checklist for Resolution:

-   Verify that the baseUrl is properly provided in the SDK initialization.
-   Confirm that the customerData is correctly set.
-   Check if the licenseKey is valid and correctly associated:
-   Ensure the licenseKey matches the Application ID registered with SEON.
-   The Application ID used in your project must be the same as the one associated with the license key on SEON’s server.
-   Ensure that the device has a stable internet connection.


## ERROR_CODE_3

### Description:

The session has expired.

### Typical Cause:

-   Sessions can expire if the verification flow takes an excessively long time to complete.

### Resolution:

-   Restart the verification by creating a new session.


## ERROR_CODE_4

### Description:

The provided license key is invalid.

### Resolution:

-   Verify that the licenseKey entered is correct.
-   Ensure the license key is active and has not been revoked or misconfigured.


## ERROR_CODE_5

### Description:

Retry limit reached during document capture.

### Typical Cause:

-   The user has failed document capture attempts more than the allowed number of retries.

### Resolution:

-   Restart the verification process by initiating a new session.


## ERROR_CODE_6

### Description:

Retry limit reached during the liveness verification step.

### Typical Cause:

-   The user has exceeded the maximum allowed number of retries for the liveness verification check.

### Resolution:
-   Restart the verification process by initiating a new session.

