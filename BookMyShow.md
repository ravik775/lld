## General Requirements
- The system should allow users to browse movies, theaters, and available showtimes based on location.
- The system should support multiple cities, each with theaters that have screens and seat configurations.
- The system should provide a flexible pricing structure based on seat types and other factors like peak hours.
- The system should handle different payment methods and notify users upon successful booking, cancellation, and payment status updates.

## Class Diagram for Ticketing Solution

### City
- **Attributes:**
  - `name: str`
  - `state: str`
  - `country: str`
  - `zipCode: str`
  - `id: Identifier`

### User
- **Attributes:**
  - `name: str`
  - `email: Email`
  - `phone: str`

### RegularUser (inherits from User)
- **Attributes and Methods:** Specific attributes or methods for regular users.

### PremiumUser (inherits from User)
- **Attributes and Methods:** Specific attributes or methods for premium users.

### Theater
- **Attributes:**
  - `id: Identifier`
  - `name: str`
  - `city: City`
  - `address: str`
  - `screens: List<Screen>`
- **Relations:**
  - `Theater *..1 City`
  - `Theater n.. Screen`

### ScreenAttribute (Enum)
- **Values:**
  - `2D`
  - `3D`
  - `DOLBY_IMAX`
  - `DOLBY_VISION`

### Screen
- **Attributes:**
  - `id: Identifier`
  - `name: str`
  - `seats: Map<SeatType, List<Seat>>`
  - `state: Enum (Operational, Closed)`
  - `features: List<ScreenAttribute>`
- **Relations:**
  - `Screen 1..* Seat`
  - `Screen 1..* ScreenAttribute`

### SeatType (Enum)
- **Values:**
  - `PLATINUM`
  - `GOLD`
  - `SILVER`

### Seat
- **Attributes:**
  - `id: Identifier`
  - `seatNumber: str`
  - `seatType: SeatType`
- **Relations:**
  - `Seat 1..1 SeatType`

### MovieAttribute (Enum)
- **Values:**
  - `2D`
  - `3D`
  - `DOLBY_IMAX`
  - `DOLBY_VISION`

### Movie
- **Attributes:**
  - `id: Identifier`
  - `name: str`
  - `attribute: MovieAttribute`
  - `cast: List<str>`
  - `language: str`
- **Relations:**
  - `Movie *..1 MovieAttribute`

### Ratings
- **Attributes:**
  - `movie: Movie`
  - `rating: int (0..5)`
  - `comments: str`
  - `user: User`

### Show
- **Attributes:**
  - `id: Identifier`
  - `screen: Screen`
  - `startTime: DateTime`
  - `endTime: DateTime`
  - `movie: Movie`
  - `showStatus: Enum (Scheduled, Cancelled, Completed)`
- **Relations:**
  - `Show *..1 Movie`
  - `Show *..1 Screen`

### ShowSeatBooking
- **Attributes:**
  - `id: Identifier`
  - `show: Show`
  - `seat: Seat`
  - `status: Enum (Booked, Available)`
  - `price: int`
- **Relations:**
  - `ShowSeatBooking *..1 Show`
  - `ShowSeatBooking *..1 Seat`

### Ticket
- **Attributes:**
  - `id: Identifier`
  - `show: Show`
  - `seats: List<ShowSeatBooking>`
  - `amount: int`
  - `user: User`
  - `bookingTime: Date`
  - `status: Enum (Confirmed, Cancelled)`
- **Relations:**
  - `Ticket *..1 Show`
  - `Ticket *..1 User`
  - `Ticket *..* ShowSeatBooking`

### PaymentMethod (Enum)
- **Values:**
  - `CREDIT_CARD`
  - `PAYPAL`
  - `DEBIT_CARD`

### Payment
- **Attributes:**
  - `id: Identifier`
  - `ticket: Ticket`
  - `paymentMethod: PaymentMethod`
  - `status: Enum (Pending, Completed, Failed)`
  - `reference: str`
- **Relations:**
  - `Payment 1..1 Ticket`

### INotificationService (Interface)
- **Methods:**
  - `notify(userId: int, ticketId: str, type: str): boolean`

### EmailNotificationService (Implements INotificationService)

### SMSNotificationService (Implements INotificationService)

### ISeatPriceComponent (Interface)
- **Methods:**
  - `getPrice(): int`

### SingleSeatPrice (Implements ISeatPriceComponent)
- **Attributes:**
  - `seatType: SeatType`
  - `basePrice: int`

### CompositeSeatPrice (Implements ISeatPriceComponent)
- **Attributes:**
  - `seatPrices: List<ISeatPriceComponent>`

### IPaymentGateway (Interface)
- **Methods:**
  - `processPayment(reference: str, amount: int): boolean`

### PaymentGateway (Abstract Class, Implements IPaymentGateway)
- **Attributes:**
  - `id: Identifier`

### CreditCardPaymentGateway (Inherits from PaymentGateway)

### PayPalPaymentGateway (Inherits from PaymentGateway)

### IPricingStrategy (Interface)
- **Methods:**
  - `getPricing(seatType: SeatType, show: Show): int`

### RegularPricingStrategy (Implements IPricingStrategy)

### HolidayPricingStrategy (Implements IPricingStrategy)

### PricingContext
- **Attributes:**
  - `pricingStrategy: IPricingStrategy`
- **Methods:**
  - `setPricingStrategy(strategy: IPricingStrategy)`
  - `calculatePrice(seatType: SeatType, show: Show): int`

### BookingService
- **Attributes:**
  - `pricingContext: PricingContext`
  - `paymentGateway: IPaymentGateway`
  - `notificationService: INotificationService`
- **Methods:**
  - `bookTicket(numberOfSeats: int, userId: int, movieId: int, showId: int): Ticket`
  - `listMovies(cityId: int): List<Movie>`

### PaymentService
- **Attributes:**
  - `paymentGateway: IPaymentGateway`
- **Methods:**
  - `pay(reference: str, amount: int): boolean`
