### Flask design is heavily context based

There are two main contexts in place when a Request is processed. They _both are up on a per-request basis_:
- `Flask.request_context`
    - HTTP request data
    - HTTP session data
    - its life-span is from `before_request` to `after_request`
- `Flask.app_context`
    - handles setup/DB connections
    - handles object chaching
    - its life-span is between set-up and tear-down
- (a test context) `Flask.test_request_context`

### Contexts are stacks (can be pushed to them)

Contexts can be used as stacks:
- `Flask._request_ctx_stack`
- `Flask._app_ctx_stack`

