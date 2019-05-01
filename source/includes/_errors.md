# Errors

The Snappr API uses the following error codes:

| Status | Name                | Description                                                                                                                                                            |
| ------ | ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 400    | SyntaxError         | Request body has incorrect formatting. Most likely invalid JSON.                                                                                                       |
| 400    | InvalidAPIVersion   | Requested an invalid API version.                                                                                                                                      |
| 400    | UnsupportedVersion  | Requested an API version that is unsupported by the requested route.                                                                                                   |
| 400    | NotImplemented      | Requested a feature that is not currently implemented.                                                                                                                 |
| 400    | ValidationError     | Validation failure. See 'Problems' field in response.                                                                                                                  |
| 400    | Conflict            | Request has a conflict with existing data.                                                                                                                             |
| 400    | NotAvailable        | Requested resource is not available in your region or for the requested date/time.                                                                                     |
| 400    | NoCoverage          | There is no coverage by Snappr on the requested `latitude`/`longitude` for the `shoottype`.                                                                            |
| 401    | Unauthorized        | Provided access token is invalid or does not have access to requested resource.                                                                                        |
| 402    | InsufficientCredits | The account does not have a sufficient credit balance to perform the request. Add more credits in the GUI.                                                             |
| 404    | NotFound            | Requested resource not found.                                                                                                                                          |
| 429    | RateLimit           | The rate limit of the provided access_token has been reached. Please have your application respect the X-RateLimit-Remaining header that is included on API responses. |
| 500    | ServerError         | There was an issue with our server. Try again later.                                                                                                                   |
| 400    | UnknownError        | An error occurred that is not a server error and is not described here.                                                                                                |
