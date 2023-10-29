# Project: Medusa Plugin StreamEstate

**Stream**Estate is a decentralized platform that aims to revolutionize the real estate rental industry by leveraging blockchain technology. The platform connects house and land owners with potential renters, providing a secure and transparent way to manage rental agreements and payments.

## Features

- **Solana Programs (Smart Contracts) Incl**:
  - Process: Automate the rental process and ensure that the rental agreement is adhered to.
  - Buyer, Seller, and Arbiter: The three parties involved in the escrow. They interact with the user interface to initiate and complete transactions.
  - Product/Service Provider: Service Provides the product or service that is being transacted. Interacts with the buyer and seller to fulfill the transaction.
  - Legal Documentations, Contracts & Agreements: Manages legal documents related to the transaction, such as contracts and agreements. Interacts with the smart contract and user interface to provide legal documents to 
    the buyer, seller, and arbiter.
  - Cryptocurrency / Stablecoin Programs: interacts with the smart contract to handle payments in stablecoins and Solana-based tokens or cryptocurrencies.
  - Transactions: Handles bank transactions for payments that are not made in SPL tokens or stablecoins. Interacts with the payment gateways, smart contract, and StreamPayments user interface to facilitate bank 
    transactions.
- **User-Friendly Interface**: Easily list properties for rent and browse available properties.
- **Dispute Resolution System**: In case of disputes, an arbiter can make a decision based on the rental agreement.
- **Review System**: Renters and property owners can leave reviews for each other.
- **Integration with External Systems**: Supports StreamPay and other payment gateways, identity verification services, and legal document management systems.
- **Support for SPL Tokens and Stablecoins**: Flexible and convenient payment options.
- Wallets: Wallets that hold the SPL tokens and EURC stablecoins. Interacts with the SPL token contract and EURC stablecoin contract to facilitate token transfers.

## Getting Started

### Prerequisites

- Node.js
- Yarn
- MedusaJs

### Installation

1. Clone the repository:

```bash
git clone https://github.com/yourusername/StreamEstate.git
```

2. Install the dependencies:

```bash
yarn install
```

3. Start the development server:

```bash
yarn dev
```

4. Open your browser and navigate to `http://localhost:3000`.

## Usage

1. List a property for rent:
   - Navigate to the "List Property" page and fill out the form with the property details.

2. Browse available properties:
   - Navigate to the "Browse Properties" page to see a list of available properties.

3. Book a property:
   - Click on a property to see the details and click the "Book" button to make a reservation.

4. Leave a review:
   - After the rental period is over, leave a review for the property owner.

## StreamEstate MedusaJs Plugin

To create a StreamEstate MedusaJs Plugin with a Rental (Contract) Plugin, you will need to create the following files:

1. `src/service/rental-agreements.ts`:
```typescript
import { BaseService } from "medusa-interfaces";
import { RentalAgreement } from "../models/rental-agreements";

class RentalAgreementService extends BaseService {
  constructor({ rentalAgreementRepository }) {
    super();

    this.rentalAgreementRepository = rentalAgreementRepository;
  }

  async create(data) {
    const rentalAgreement = await this.rentalAgreementRepository.create(data);
    return rentalAgreement;
  }

  async retrieve(id) {
    const rentalAgreement = await this.rentalAgreementRepository.findOne({ id });
    return rentalAgreement;
  }

  async update(id, data) {
    const rentalAgreement = await this.rentalAgreementRepository.update({ id }, data);
    return rentalAgreement;
  }

  async delete(id) {
    const result = await this.rentalAgreementRepository.delete({ id });
    return result;
  }
}

export default RentalAgreementService;
```

2. `models/rental-agreements.ts`:
```typescript
import { Entity, Column, PrimaryGeneratedColumn } from "typeorm";

@Entity()
export class RentalAgreement {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  propertyId: number;

  @Column()
  renterId: number;

  @Column()
  ownerId: number;

  @Column()
  rentAmount: number;

  @Column()
  startDate: Date;

  @Column()
  endDate: Date;

  @Column({ default: false })
  isPaid: boolean;
}
```

3. `repositories/rental-agreements.ts`:
```typescript
import { EntityRepository, Repository } from "typeorm";
import { RentalAgreement } from "../models/rental-agreements";

@EntityRepository(RentalAgreement)
export class RentalAgreementRepository extends Repository<RentalAgreement> {
  async create(data) {
    const rentalAgreement = this.create(data);
    await this.save(rentalAgreement);
    return rentalAgreement;
  }

  async update(criteria, data) {
    const rentalAgreement = await this.findOne(criteria);
    if (!rentalAgreement) {
      throw new Error("Rental agreement not found");
    }

    this.merge(rentalAgreement, data);
    await this.save(rentalAgreement);
    return rentalAgreement;
  }

  async delete(criteria) {
    const rentalAgreement = await this.findOne(criteria);
    if (!rentalAgreement) {
      throw new Error("Rental agreement not found");
    }

    await this.remove(rentalAgreement);
    return true;
  }
}
```

## Contributing

Contributions are welcome! Please feel free to submit a pull request or open an issue.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
