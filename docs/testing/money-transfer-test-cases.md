# Money Transfer Test Cases

## Scope and Assumptions

Status: Draft — the rules below are proposed portfolio requirements.

These cases cover form validation, transfer submission, balance changes,
and history entries. Final transaction status and processing-time
requirements are not yet defined.

Proposed rules:

- Transfers require a selected card, a recipient, and a positive amount.
- The amount must not exceed the available card balance.
- Transfers have no additional fee.
- Cancelling before submission does not change the balance or history.

These rules are consistent with observations but still require acceptance
as project requirements. They are not an external banking specification.

Each case requires its preconditions to be established independently.
Recipients are predefined demo contacts. No unrelated transactions should
occur during execution.

Future execution results will be recorded separately in `test-runs/`.

## Test Cases

```gherkin
Feature: Money transfers
  Users can send available funds to a selected recipient.

  Background:
    Given the user is logged in
    And the money transfer form is open

  @TR-001
  Scenario: No cards available
    Given the user has no cards
    When the user opens card selection
    Then the message "You have no added cards" should be displayed
    And no source card should be available for selection

  @TR-002
  Scenario: Card has no available funds
    Given the user has a card with a balance of 0.00 USD
    When the user selects that card
    Then the message "Insufficient card balance" should be displayed
    And the Proceed button should be disabled

  @TR-003
  Scenario: Zero transfer amount
    Given a card with a balance of 250.00 USD is selected
    And a recipient is selected
    When the user sets the transfer amount to 0.00 USD
    Then the Proceed button should be disabled

  @TR-004
  Scenario: Transfer one cent
    Given the user has exactly one card with a balance of 250.00 USD
    And that card is selected
    And the selected recipient is Andi Taher
    And the initial transaction history is known
    When the user submits a transfer of 0.01 USD
    Then a submission confirmation should be displayed
    And the card balance should become 249.99 USD
    And the "My Balance" value on the home screen should become 249.99 USD
    And transaction history should contain exactly one new outgoing transfer of 0.01 USD to Andi Taher

  @TR-005
  Scenario: Increasing the amount at the balance limit
    Given a card with a balance of 250.00 USD is selected
    And a recipient is selected
    And the transfer amount is 250.00 USD
    When the user clicks the increase amount button
    Then the transfer amount should remain 250.00 USD
    And the Proceed button should remain enabled

  @TR-006
  Scenario: Cancel before submission
    Given a card with a balance of 249.99 USD is selected
    And a recipient is selected
    And the transfer amount is 50.00 USD
    And the initial transaction history is known
    When the user cancels before submitting
    Then the transfer form should close
    And the card balance should remain 249.99 USD
    And transaction history should remain unchanged

  @TR-007
  Scenario: Transfer the entire available balance
    Given the user has exactly one card with a balance of 249.99 USD
    And that card is selected
    And the selected recipient is Yulisa Meyun
    And the initial transaction history is known
    When the user submits a transfer of 249.99 USD
    Then a submission confirmation should be displayed
    And the card balance should become 0.00 USD
    And the "My Balance" value on the home screen should become 0.00 USD
    And transaction history should contain exactly one new outgoing transfer of 249.99 USD to Yulisa Meyun
```

## Execution Notes

- For manual checks, dismiss the submission confirmation with Continue
  before navigating to the home screen or history.
- Compare history with its recorded initial state to identify new entries.
- Submission confirmation does not establish final transaction completion.
- Timing and final-status assertions will be added after their requirements
  are clarified.

## Observations and Open Questions

- Clicking plus once at zero produced USD 0.01.
- Tapping the displayed amount did not open a keyboard.
- The maximum preset adapted to the selected card balance.
- For the full-balance transfer, the balance was reduced while a yellow
  hourglass was visible. The icon later changed to a green check mark.
- Exact status definitions and processing time remain unconfirmed.
- Fee, rounding, and precision rules require clarification.

## Remaining Coverage

- Repeated submission and duplicate prevention.
- Recipient validation and changing cards.
- Invalid amounts at the logic level.
- Rounding and precision across multiple operations.
- Failed pending transactions and balance recovery.

TR-005 verifies the UI limit only. It does not prove that the underlying
logic rejects an amount exceeding the balance.

Priorities and planned automation levels will be assigned during M1.