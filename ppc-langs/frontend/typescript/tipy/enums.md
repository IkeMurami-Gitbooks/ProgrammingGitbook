# Enums

```typescript
// enum по умолчанию number
enum Membership {
    Simple,
    Standart,
    Premium
}

const membership = Membership.Standart
// Reverse Enum
const membershipReverse = Membership[membership]  // 'Standart'



enum SocialMedia {
    VK = 'VK',
    FACEBOOK = 'FACEBOOK'
}
```
