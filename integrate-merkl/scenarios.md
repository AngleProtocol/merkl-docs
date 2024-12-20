# Scenarios

## Retrieve the APR of an opportunity

### 1. Retrieve the opportunity id

The first step to obtaining the APR of an opportunity is to retrieve its unique identifier. You can accomplish this by searching for the desired opportunity using the `GET /v4/opportunities/` endpoint.

<aside>
💡

Each returned opportunity has an `id` attribute. It is a numeric string you can use to uniquely identify an opportunity.

</aside>

### 2. Retrieve the APR

To retrieve the details of a specific opportunity, use the `GET /v4/opportunities/{id}` endpoint, replacing `{id}` with the opportunity's unique identifier. The response includes all relevant opportunity data, including the APR, which is provided under the `apr` attribute.
