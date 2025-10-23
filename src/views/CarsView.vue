<template>
  <div class="cars-page">
    <div class="container">
      <div class="page-header">
        <h1 class="page-title">
          Browse <span class="gradient-text">Premium</span> Cars
        </h1>
        <p class="page-subtitle">Find the perfect car for your journey from our curated collection</p>
      </div>

      <!-- Filters -->
      <div class="filters-section">
        <div class="filters">
          <div class="filter-group">
            <label class="filter-label">Brand</label>
            <select v-model="filters.brand" class="filter-select">
              <option value="">All Brands</option>
              <option value="Toyota">Toyota</option>
              <option value="Honda">Honda</option>
              <option value="BMW">BMW</option>
              <option value="Mercedes">Mercedes</option>
              <option value="Audi">Audi</option>
            </select>
          </div>

          <div class="filter-group">
            <label class="filter-label">Type</label>
            <select v-model="filters.type" class="filter-select">
              <option value="">All Types</option>
              <option value="Sedan">Sedan</option>
              <option value="SUV">SUV</option>
              <option value="Luxury">Luxury</option>
              <option value="Economy">Economy</option>
            </select>
          </div>

          <div class="filter-group">
            <label class="filter-label">Price Range</label>
            <select v-model="filters.priceRange" class="filter-select">
              <option value="">Any Price</option>
              <option value="0-50">$0 - $50</option>
              <option value="50-100">$50 - $100</option>
              <option value="100-200">$100 - $200</option>
              <option value="200+">$200+</option>
            </select>
          </div>

          <button @click="clearFilters" class="btn btn-secondary">
            <span>Clear Filters</span>
            <svg class="btn-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
            </svg>
          </button>
        </div>
      </div>

      <!-- Cars Grid -->
      <div class="cars-grid">
        <div class="car-card" v-for="car in filteredCars" :key="car.id" @click="viewCar(car.id)">
          <div class="car-image">
            <div class="car-placeholder">{{ getCarEmoji(car.brand) }}</div>
            <div class="car-overlay">
              <div class="car-badge">{{ getCarBadge(car.price) }}</div>
            </div>
          </div>
          <div class="car-info">
            <div class="car-header">
              <h3 class="car-name">{{ car.brand }} {{ car.model }}</h3>
              <div class="car-rating">
                <span class="stars">★★★★★</span>
                <span class="rating-text">4.9</span>
              </div>
            </div>
            <p class="car-type">{{ car.type }}</p>
            <div class="car-features-list">
              <span class="feature-tag" v-for="feature in car.features.split(', ')" :key="feature">{{ feature }}</span>
            </div>
            <div class="car-footer">
              <div class="car-price">
                <span class="price-amount">${{ car.price }}</span>
                <span class="price-period">/day</span>
              </div>
              <button class="btn btn-primary">
                <span>View Details</span>
                <svg class="btn-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7l5 5m0 0l-5 5m5-5H6"/>
                </svg>
              </button>
            </div>
          </div>
        </div>
      </div>

      <div v-if="filteredCars.length === 0" class="no-results">
        <div class="no-results-icon">🚗</div>
        <h3>No cars found</h3>
        <p>Try adjusting your filters to find more options</p>
        <button @click="clearFilters" class="btn btn-primary">
          <span>Clear All Filters</span>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const filters = ref({
  brand: '',
  type: '',
  priceRange: '',
})

const cars = ref([
  {
    id: 1,
    brand: 'Toyota',
    model: 'Camry',
    type: 'Sedan',
    price: 45,
    features: 'Automatic, 4 Seats, Air Conditioning',
    available: true,
  },
  {
    id: 2,
    brand: 'Honda',
    model: 'Civic',
    type: 'Sedan',
    price: 40,
    features: 'Manual, 4 Seats, Bluetooth',
    available: true,
  },
  {
    id: 3,
    brand: 'BMW',
    model: 'X5',
    type: 'SUV',
    price: 120,
    features: 'Automatic, 7 Seats, GPS, Leather',
    available: true,
  },
  {
    id: 4,
    brand: 'Mercedes',
    model: 'C-Class',
    type: 'Luxury',
    price: 150,
    features: 'Automatic, 4 Seats, Premium Sound',
    available: true,
  },
  {
    id: 5,
    brand: 'Audi',
    model: 'A4',
    type: 'Luxury',
    price: 130,
    features: 'Automatic, 4 Seats, Quattro AWD',
    available: true,
  },
  {
    id: 6,
    brand: 'Toyota',
    model: 'RAV4',
    type: 'SUV',
    price: 65,
    features: 'Automatic, 5 Seats, All-Wheel Drive',
    available: true,
  },
  {
    id: 7,
    brand: 'Honda',
    model: 'Accord',
    type: 'Sedan',
    price: 50,
    features: 'Automatic, 4 Seats, Hybrid',
    available: true,
  },
  {
    id: 8,
    brand: 'BMW',
    model: '3 Series',
    type: 'Luxury',
    price: 110,
    features: 'Automatic, 4 Seats, Sport Package',
    available: true,
  },
])

const filteredCars = computed(() => {
  return cars.value.filter((car) => {
    const brandMatch = !filters.value.brand || car.brand === filters.value.brand
    const typeMatch = !filters.value.type || car.type === filters.value.type
    const priceMatch =
      !filters.value.priceRange || checkPriceRange(car.price, filters.value.priceRange)

    return brandMatch && typeMatch && priceMatch
  })
})

const checkPriceRange = (price, range) => {
  switch (range) {
    case '0-50':
      return price <= 50
    case '50-100':
      return price > 50 && price <= 100
    case '100-200':
      return price > 100 && price <= 200
    case '200+':
      return price > 200
    default:
      return true
  }
}

const clearFilters = () => {
  filters.value = {
    brand: '',
    type: '',
    priceRange: '',
  }
}

const viewCar = (carId) => {
  router.push(`/car/${carId}`)
}

const getCarEmoji = (brand) => {
  const emojiMap = {
    'Toyota': '🚗',
    'Honda': '🚙',
    'BMW': '🚘',
    'Mercedes': '🚖',
    'Audi': '🚔',
    'Tesla': '⚡'
  }
  return emojiMap[brand] || '🚗'
}

const getCarBadge = (price) => {
  if (price <= 50) return 'Economy'
  if (price <= 100) return 'Popular'
  if (price <= 200) return 'Premium'
  return 'Luxury'
}
</script>

<style scoped>
.cars-page {
  margin-top: 70px;
  padding: 40px 20px;
  min-height: 100vh;
  background: #f8fafc;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
}

.page-header {
  text-align: center;
  margin-bottom: 3rem;
}

.page-header h1 {
  font-size: 2.5rem;
  color: #1f2937;
  margin-bottom: 1rem;
}

.page-header p {
  color: #6b7280;
  font-size: 1.1rem;
}

.filters {
  background: white;
  padding: 2rem;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  margin-bottom: 2rem;
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
  align-items: end;
}

.filter-group {
  display: flex;
  flex-direction: column;
  min-width: 150px;
}

.filter-group label {
  margin-bottom: 0.5rem;
  color: #374151;
  font-weight: 500;
}

.filter-group select {
  padding: 8px 12px;
  border: 1px solid #d1d5db;
  border-radius: 6px;
  font-size: 14px;
}

.btn {
  display: inline-flex;
  align-items: center;
  gap: 0.375rem;
  padding: 0.375rem 0.75rem;
  border-radius: 6px;
  text-decoration: none;
  font-weight: 600;
  font-size: 0.8rem;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
  cursor: pointer;
  border: none;
  min-height: 32px;
}

.btn-primary {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.3);
}

.btn-primary:hover {
  transform: translateY(-1px);
  box-shadow: 0 8px 20px rgba(102, 126, 234, 0.4);
}

.btn-secondary {
  background: #6b7280;
  color: white;
  box-shadow: 0 4px 12px rgba(107, 114, 128, 0.3);
}

.btn-secondary:hover {
  background: #4b5563;
  transform: translateY(-1px);
  box-shadow: 0 8px 20px rgba(107, 114, 128, 0.4);
}

.btn-icon {
  width: 14px;
  height: 14px;
  transition: transform 0.3s;
}

.btn:hover .btn-icon {
  transform: translateX(2px);
}

.cars-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 2rem;
}

.car-card {
  background: white;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: all 0.3s;
  cursor: pointer;
}

.car-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
}

.car-image {
  height: 200px;
  background: #f3f4f6;
  display: flex;
  align-items: center;
  justify-content: center;
}

.car-placeholder {
  font-size: 4rem;
  opacity: 0.7;
}

.car-info {
  padding: 1.5rem;
}

.car-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 0.5rem;
}

.car-name {
  font-size: 1.25rem;
  font-weight: 700;
  color: #1a202c;
  margin: 0;
}

.car-rating {
  display: flex;
  align-items: center;
  gap: 0.25rem;
}

.stars {
  color: #fbbf24;
  font-size: 0.875rem;
}

.rating-text {
  font-size: 0.875rem;
  color: #64748b;
  font-weight: 600;
}

.car-type {
  color: #64748b;
  margin-bottom: 1rem;
  font-size: 0.875rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.car-features-list {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}

.feature-tag {
  background: #f1f5f9;
  color: #475569;
  padding: 0.25rem 0.75rem;
  border-radius: 12px;
  font-size: 0.75rem;
  font-weight: 500;
}

.car-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.car-price {
  display: flex;
  align-items: baseline;
  gap: 0.25rem;
}

.price-amount {
  font-size: 1.5rem;
  font-weight: 800;
  color: #059669;
}

.price-period {
  font-size: 0.875rem;
  color: #64748b;
}

.no-results {
  text-align: center;
  padding: 4rem;
  color: #6b7280;
}

.no-results h3 {
  margin-bottom: 1rem;
  color: #374151;
}

@media (max-width: 768px) {
  .filters {
    flex-direction: column;
    align-items: stretch;
  }

  .filter-group {
    min-width: auto;
  }

  .cars-grid {
    grid-template-columns: 1fr;
  }
}
</style>
