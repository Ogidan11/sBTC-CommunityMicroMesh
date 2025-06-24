
#  Community Micro-Investment Smart Contract

This smart contract enables decentralized, community-driven funding for micro-projects on the Stacks blockchain. It allows users to propose new projects, invest STX tokens in projects they support, vote on project viability, and finalize outcomes based on democratic consensus.

## 📌 Features

* ✅ **Create Projects**: Users can propose new community projects with a title, description, and funding goal.
* 💰 **Invest in Projects**: Community members can contribute STX within defined limits.
* 🗳️ **Voting Mechanism**: Investors can vote to approve or reject a project after the funding deadline.
* 📊 **Finalization Logic**: Projects are finalized based on votes once the deadline passes.
* 🔐 **Comprehensive Validation**: All inputs are stringently validated to ensure data quality and security.

---

##  Contract Architecture

### 🔒 Constants

| Constant                     | Description                                |
| ---------------------------- | ------------------------------------------ |
| `contract-owner`             | Deployer of the contract                   |
| `min-investment`             | 1 STX minimum                              |
| `max-investment`             | 1000 STX maximum                           |
| `voting-period`              | \~1 day (144 blocks)                       |
| `min-vote-threshold`         | 500,000 votes required to validate outcome |
| `min/max-title-length`       | Enforces quality titles                    |
| `min/max-description-length` | Enforces quality descriptions              |

### ❗ Error Codes

| Code | Meaning                   |
| ---- | ------------------------- |
| 401  | Not authorized            |
| 402  | Invalid investment amount |
| 404  | Project not found         |
| 405  | Project expired           |
| 406  | Already voted             |
| 407  | Invalid title             |
| 408  | Invalid description       |
| 409  | Invalid project ID        |
| 410  | Max investment exceeded   |
| 411  | Zero amount               |
| 412  | Project inactive          |
| 413  | Duplicate title           |

---

## 🧠 Data Model

### Project

```clarity
{
  project-id: uint,
  creator: principal,
  title: string-ascii (max 50),
  description: string-ascii (max 500),
  funding-goal: uint,
  current-amount: uint,
  status: "active" | "approved" | "rejected" | "failed",
  end-block: uint,
  creation-block: uint,
  last-updated: uint
}
```

### Investment

Each investor has a record per project:

```clarity
{
  amount: uint,
  timestamp: uint,
  last-updated: uint
}
```

### Votes

```clarity
{
  vote: bool,
  timestamp: uint
}
```

### Vote Counts

```clarity
{
  total-votes: uint,
  positive-votes: uint,
  negative-votes: uint,
  last-updated: uint
}
```

---

## 🔧 Public Functions

### 1. `create-project`

Create a new project.

```clarity
(define-public (create-project (title ...) (description ...) (funding-goal ...)))
```

### 2. `invest`

Invest in a project.

```clarity
(define-public (invest (project-id ...) (amount ...)))
```

### 3. `vote`

Vote on a project you’ve invested in.

```clarity
(define-public (vote (project-id ...) (vote-value bool)))
```

### 4. `finalize-project`

Finalize the status after the voting deadline.

```clarity
(define-public (finalize-project (project-id ...)))
```

---

## 🔍 Read-Only Functions

* `get-project`: Retrieve project data.
* `get-investment`: View your investment in a project.
* `get-vote`: See your vote status on a project.
* `get-vote-counts`: Track vote statistics for a project.

---

## 🛡️ Validation Rules

* **Project titles and descriptions** must meet minimum/maximum lengths.
* **Investment amounts** must fall within defined boundaries.
* **Duplicate titles** are not allowed.
* **Projects** must be active and unexpired to receive investments or votes.

---

## 🧪 Example Flow

1. **Alice** creates a project titled *"Clean Water for Village Z"*.
2. **Bob and Carol** invest 50 STX and 200 STX, respectively.
3. After the voting period, both vote “yes”.
4. **Voting succeeds**, and the project is marked **approved**.

---

##  Deployment

To deploy:

```bash
clarinet check
clarinet test
clarinet deploy
```

---
