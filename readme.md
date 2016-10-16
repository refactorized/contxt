## contxt

human context logging and reporting

### goals
  - api
    - record new event -> processing
    - list recent records
  - design parameters
    - parse bots (see below)
    - parse-ables do not span messages
    - messages can be referenceds
    - parse-bots can maintain state
      - must identify, rectify loss-of-state
      - must respond to reset event
      - should respond on meta channel
    - channels
      - allow full-text channels for messages and responses
      - vast majority of messages are on main channel
      - useful error output, useful annotations, etc.
      - optional / various views in review UI
  - parsers and parse-bots
    - not needed for main purpose (self recording)
    - can be activated on existing logs
    - parsers
      - are stateless
      - do not span messages
      - therefore can be materialized from any given event
      - OR range of events, in aggregate
      - are either map or reduce
    - parse-bots
      - implement parsers, as a trigger and input source
      - are stateful
      - may only act on date and text of message
        - any external data must be passed through the message
      - therefore are deterministic given any date range, and message history
      - will change, and must be recalculated if the message history is changed
      - can only advance state during:
        - the processing of a message caught by one if its parsers
        - some, as yet defined, _system lifetime events_.
