# STX Proof of Attendance (POA) Smart Contract

## Overview
The **STX Proof of Attendance (POA)** smart contract enables event organizers to issue attendance-based rewards on the Stacks blockchain. Attendees can check in, check out, have their attendance verified, and claim rewards upon successful verification.

This contract supports multiple events, verifiers, and reward structures, ensuring a robust and scalable POA system.

## Features
- **Event Creation & Management**: Organizers can create events with specific durations, rewards, and minimum attendance requirements.
- **Attendance Tracking**: Participants can check in and check out of events.
- **Verification System**: Authorized verifiers can validate attendance records.
- **Reward Distribution**: Verified attendees can claim STX token rewards.
- **Treasury Management**: The contract manages funds for event rewards.
- **Admin Controls**: The contract owner can add verifiers, deposit and withdraw funds, and deactivate events.

---

## Smart Contract Functions
### Read-Only Functions
#### 1. `get-owner()`
- Returns the contract owner.

#### 2. `get-event(event-id)`
- Fetches details of an event by `event-id`.

#### 3. `get-attendance-record(event-id, attendee)`
- Retrieves an attendee’s check-in and check-out details for a specific event.

#### 4. `get-reward-claim(event-id, attendee)`
- Checks if an attendee has already claimed their reward.

#### 5. `is-verifier(address)`
- Returns `true` if an address is an authorized verifier.

#### 6. `event-exists(event-id)`
- Checks if an event exists.

#### 7. `can-verify-attendance(event-id, attendee)`
- Determines if an attendee’s attendance can be verified.

#### 8. `get-verification-details(event-id, attendee)`
- Retrieves verification details of an attendee.

#### 9. `get-full-verification-status(event-id, attendee)`
- Returns the verification status and details of an attendee.

---

### Public Functions
#### 1. `create-event(name, description, start-height, duration, base-reward, bonus-reward, min-attendance)`
- **Creates a new event.**
- Validates input parameters and assigns a unique `event-id`.
- Requires the caller to be the contract owner.

#### 2. `check-in(event-id)`
- Allows an attendee to check into an event.
- Ensures the event is active and has started.

#### 3. `check-out(event-id)`
- Allows an attendee to check out from an event.
- Updates attendance duration.

#### 4. `verify-attendance(event-id, attendee)`
- Verifiers validate an attendee's presence at the event.
- Ensures attendance records exist before verification.

#### 5. `claim-reward(event-id)`
- **Allows verified attendees to claim their STX reward.**
- Calculates the reward based on attendance duration.
- Transfers funds from the treasury balance.

#### 6. `add-verifier(address)`
- **Adds a new verifier.**
- Requires contract owner authorization.

#### 7. `remove-verifier(address)`
- **Removes an existing verifier.**
- Requires contract owner authorization.

#### 8. `deactivate-event(event-id)`
- **Deactivates an active event.**
- Requires contract owner authorization.

#### 9. `deposit-funds(amount)`
- **Adds STX tokens to the contract treasury.**

#### 10. `withdraw-funds(amount)`
- **Withdraws STX tokens from the contract treasury.**
- Requires contract owner authorization.

---

## Error Codes
The contract defines several error codes to handle invalid operations:
| Error Code | Description |
|------------|-------------|
| `u100` | Not authorized |
| `u101` | Already claimed |
| `u102` | Event not ended |
| `u103` | Event ended |
| `u104` | No reward |
| `u105` | Event not found |
| `u106` | Insufficient funds |
| `u107` | Invalid duration |
| `u108` | Already registered |
| `u110` | Invalid start height |
| `u111` | Invalid reward |
| `u112` | Invalid min attendance |
| `u120` | Event not active |
| `u121` | No check-in record |
| `u122` | Already verified |
| `u123` | Invalid attendee |
| `u1002` | Invalid address |
| `u1003` | Already a verifier |
| `u1004` | Not a verifier |
| `u1005` | Invalid amount |
| `u1006` | Event already inactive |
| `u1007` | Transfer failed |

---

## Contract Data Variables
| Variable | Description |
|----------|-------------|
| `contract-owner` | Stores the contract owner’s principal address. |
| `event-counter` | Keeps track of the number of events created. |
| `treasury-balance` | Maintains the STX balance available for rewards. |

---

## Data Structures
### 1. `events`
Stores event details:
```lisp
{ name, description, start-height, end-height, base-reward, bonus-reward, min-attendance-duration, organizer, is-active }
```

### 2. `event-attendance`
Tracks attendees’ check-in and check-out details:
```lisp
{ event-id, attendee, check-in-height, check-out-height, duration, verified }
```

### 3. `verification-details`
Stores verifier information:
```lisp
{ event-id, attendee, verified-by, verified-at }
```

### 4. `rewards-claimed`
Records claimed rewards:
```lisp
{ event-id, attendee, amount, claimed-at, reward-tier }
```

### 5. `verifiers`
Maps verifier addresses:
```lisp
{ address: bool }
```

---

## Usage Guide
### Event Creation
```lisp
(create-event "Blockchain Meetup" "A conference on blockchain technology" u1000 u144 u100000000 u50000000 u120)
```

### Checking In
```lisp
(check-in u1)
```

### Checking Out
```lisp
(check-out u1)
```

### Verifying Attendance
```lisp
(verify-attendance u1 'SP1234567890ABCDEF)
```

### Claiming Rewards
```lisp
(claim-reward u1)
```

### Managing Verifiers
```lisp
(add-verifier 'SP9876543210FEDCBA)
(remove-verifier 'SP9876543210FEDCBA)
```

### Treasury Management
```lisp
(deposit-funds u1000000000)
(withdraw-funds u500000000)
```

---

## Security Considerations
- **Ownership and Authorization**: Only the contract owner can add verifiers, manage funds, and deactivate events.
- **Verification Requirement**: Rewards can only be claimed after verification.
- **Treasury Protection**: Rewards cannot exceed available funds.

---

## Conclusion
This STX POA smart contract enables event-based reward distribution with transparency, security, and verifiability. Organizers can create events, attendees can earn rewards, and verifiers ensure the legitimacy of participation.

