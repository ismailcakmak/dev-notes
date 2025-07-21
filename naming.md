
# Backend

Repository layer var ve burda dosya isimlendirmek istiyorsunuz diyelim :

Good naming options:
- repository.js - Generic, works well if you only have one data source
- mongoRepository.js - Specific to MongoDB, good if you might add other data sources later
- userRepository.js (or whatever your main entity is) - Entity-specific naming
- database.js - Simple and clear for database operations