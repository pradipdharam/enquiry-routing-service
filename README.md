# Enquiry Routing Service

A simple Python service that routes incoming enquiries through a chain of responsibility handlers and classifies them into one of the supported enquiry types.

## Overview

The application receives a free-text enquiry string and passes it through the following handler chain:

1. `LogHandler`
2. `AcademicEnquiryHandler`
3. `ProjectEnquiryHandler`
4. `SubscriptionEnquiryHandler`
5. `IdleHandler`

Each handler checks for specific keywords in the enquiry text:

- `AcademicEnquiryHandler`: matches `Ds Algo` or `Design` -> `EnquiryType.ACADEMIC`
- `ProjectEnquiryHandler`: matches `Project` -> `EnquiryType.PROJECT`
- `SubscriptionEnquiryHandler`: matches `Upgrade` or `payment` -> `EnquiryType.SUBSCRIPTION`
- `IdleHandler`: no match -> `EnquiryType.UNKNOWN`

The `LogHandler` prints the original enquiry and the final `EnquiryType` returned by the chain.

## Project Structure

- `handle_enquiry_api.py` - Entry point for handling enquiries.
- `enquiry/`
  - `enquiry_handler_factory.py` - Builds the handler chain.
  - `academic_enquiry_handler.py` - Handles academic-related enquiries.
  - `project_enquiry_handler.py` - Handles project-related enquiries.
  - `subscription_enquiry_handler.py` - Handles subscription or payment-related enquiries.
  - `idle_handler.py` - Default fallback handler.
  - `log_handler.py` - Logs the enquiry and result.
- `data/enquiry_type.py` - Defines the enquiry categories.

## How It Works

The factory creates the chain as:

```python
LogHandler(
    AcademicEnquiryHandler(
        ProjectEnquiryHandler(
            SubscriptionEnquiryHandler(IdleHandler())
        )
    )
)
```

When `handle_enquiry()` is called, the first handler in the chain receives the enquiry. If it cannot classify the enquiry, it delegates to the next handler.

## Running the Example

From the project root, run:

```bash
python3 handle_enquiry_api.py
```

The current sample input in the file is:

```python
"I want to take 1 month Upgrade"
```

Expected classification:

```text
EnquiryType.SUBSCRIPTION
```

## Sample Output

```text
C:\Users\Pradip\PycharmProjects\enquiry-handler_machine_coding\.venv\Scripts\python.exe C:\Users\Pradip\PycharmProjects\enquiry-handler_machine_coding\handle_enquiry_api.py 
I want to take 1 month Upgrade
Inside AcademicEnquiryHandler
Inside ProjectEnquiryHandler
Inside SubscriptionEnquiryHandler
Inside LogHandler
EnquiryType.SUBSCRIPTION

Process finished with exit code 0
```

## Notes

- The `HandleEnquiryApi.handle_enquiry()` method currently does not return the detected enquiry type; it only triggers the handler chain.
- The design follows the Chain of Responsibility pattern.

