# Clarity Coding Standards

## Table of Contents

1. [Naming Conventions](#naming-conventions)

   - [Variables](#variables)
   - [Functions](#functions)
   - [Constants](#constants)
   - [Maps](#maps)

2. [General Guidelines]()
3. [More Examples]()
   - Good
   - Bad

## Naming Conventions

#### **Variables**

- Use camelCase for `define-data-var`.
- Ensure variable names are descriptive and avoid abbreviations.

Good:

```clojure
(define-data-var voteActive bool true)
(define-data-var voteStart uint u0)
```

Bad:

```clojure
(define-data-var active bool true)
(define-data-var vote-active bool true)
(define-data-var start uint u0)
(define-data-var vote-start uint u0)
```

#### **Functions**

- Use kebab-case for `define-public`, `define-read-only`, and `define-private` functions.
- Ensure function names clearly convey their purpose.

Good:

```clojure
(define-read-only (get-user-info (sender principal))
  ;; ...
)

(define-public (execute (sender principal))
  ;; ...
)

(define-private (is-allowed (sender principal))
  ;; ...
)
```

Bad:

```clojure
(define-public (exec (sender principal))
  ;; ...
)
(define-public (executeProposal (sender principal))
  ;; ...
)
```

#### **Constants**

- Use kebab-case for `define-public`, `define-read-only`, and `define-private` functions.
- Ensure function names clearly convey their purpose.

Good:

```clojure
(define-public (execute (sender principal))
  ;; ...
)
```

Bad:

```clojure
(define-public (exec (sender principal))
  ;; ...
)
(define-public (executeProposal (sender principal))
  ;; ...
)
```

- Use UPPER_CASE with underscores for constants.

Good:

```clojure
(define-constant ERR_PANIC (err u22000))
(define-constant VOTE_SCALE_FACTOR (pow u10 u16))
```

Bad:

```clojure
(define-constant ErrPanic (err u22000))
(define-constant voteScaleFactor (pow u10 u16))
```

#### **Maps**

- Use PascalCase for map names.
- Ensure the key and value structures are clearly defined and descriptive.

Good:

```clojure
(define-map ProposalVotes
  uint
  {
    totalAmountYes: uint,
    totalAmountNo: uint,
    totalVotesYes: uint,
    totalVotesNo: uint,
  }
)
```

Bad:

```clojure
(define-map votes
  someid uint
  {
    totalAmountYes: uint,
    totalAmountNo: uint,
    totalVotesYes: uint,
    totalVotesNo: uint,
  }
)
```

## General Guidelines

- **Single Purpose Functions**: Each function should have a single responsibility.
- **Descriptive Names**: Avoid abbreviations and use descriptive names for variables, functions, and constants.
- **Commenting**: Include comments, especially for complex logic or important constants.

Good:

```clojure
(define-public (vote-on-proposal (vote bool))
  (let
    (
      (voterId (unwrap! (contract-call? .ccd003-user-registry get-user-id contract-caller) ERR_USER_NOT_FOUND))
      (voterRecord (map-get? UserVotes voterId))
    )
    ;; check if vote is active
    (asserts! (var-get voteActive) ERR_PROPOSAL_NOT_ACTIVE)
    ;; check if vote record exists for user
    (match voterRecord record
      (let
        (
          (oldVote (get vote record))
          (nycVoteAmount (get nyc record))
        )
      ;; ...
  )
)
```

Bad:

```clojure
(define-public (vote-proposal (v bool))
  (let
    (
      (vId (unwrap! (contract-call? .ccd003-user-registry get-user contract-caller) ERR_USER_NOT_FOUND))
      (vRecord (map-get? UserVotes vId))
    )
    (asserts! (var-get vActive) ERR_PROPOSAL_NOT_ACTIVE)
    (match vRecord rec
      (let
        (
          (oVote (get vote rec))
          (nVoteAmt (get nyc rec))
        )
      ;; ...
)
```

## More Examples
