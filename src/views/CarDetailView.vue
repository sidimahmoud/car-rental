<template>
  <div class="car-detail-page">
    <div class="container">
      <div v-if="car" class="car-detail">
        <div class="car-images">
          <div class="main-image">
            <div class="car-placeholder-large">🚗</div>
          </div>
        </div>

        <div class="car-info">
          <h1>{{ car.brand }} {{ car.model }}</h1>
          <p class="car-type">{{ car.type }}</p>
          <p class="car-price">${{ car.price }}/day</p>

          <div class="car-features">
            <h3>Features</h3>
            <ul>
              <li v-for="feature in car.features.split(', ')" :key="feature">{{ feature }}</li>
            </ul>
          </div>

          <div class="car-specs">
            <h3>Specifications</h3>
            <div class="specs-grid">
              <div class="spec">
                <span class="spec-label">Seats:</span>
                <span class="spec-value">{{ car.seats || '4' }}</span>
              </div>
              <div class="spec">
                <span class="spec-label">Transmission:</span>
                <span class="spec-value">{{ car.transmission || 'Automatic' }}</span>
              </div>
              <div class="spec">
                <span class="spec-label">Fuel Type:</span>
                <span class="spec-value">{{ car.fuelType || 'Gasoline' }}</span>
              </div>
              <div class="spec">
                <span class="spec-label">Mileage:</span>
                <span class="spec-value">{{ car.mileage || 'Unlimited' }}</span>
              </div>
            </div>
          </div>

          <div class="rental-section">
            <h3>Rent This Car</h3>
            <div class="rental-form">
              <div class="form-row">
                <div class="form-group">
                  <label>Pickup Date</label>
                  <input type="date" v-model="rentalData.pickupDate" :min="today" required />
                </div>
                <div class="form-group">
                  <label>Return Date</label>
                  <input
                    type="date"
                    v-model="rentalData.returnDate"
                    :min="rentalData.pickupDate"
                    required
                  />
                </div>
              </div>

              <div class="rental-summary">
                <div class="summary-row">
                  <span>Daily Rate:</span>
                  <span>${{ car.price }}</span>
                </div>
                <div class="summary-row">
                  <span>Days:</span>
                  <span>{{ rentalDays }}</span>
                </div>
                <div class="summary-row total">
                  <span>Total:</span>
                  <span>${{ totalPrice }}</span>
                </div>
              </div>

              <button
                @click="proceedToRental"
                class="btn btn-primary btn-large"
                :disabled="!canProceed"
              >
                Proceed to Rental
              </button>
            </div>
          </div>
        </div>
      </div>

      <div v-else class="loading">
        <p>Loading car details...</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'

const route = useRoute()
const router = useRouter()

const car = ref(null)
const today = new Date().toISOString().split('T')[0]

const rentalData = ref({
  pickupDate: '',
  returnDate: '',
})

const cars = [
  {
    id: 1,
    brand: 'Toyota',
    model: 'Camry',
    type: 'Sedan',
    price: 45,
    features: 'Automatic, 4 Seats, Air Conditioning, Bluetooth, GPS',
    seats: 4,
    transmission: 'Automatic',
    fuelType: 'Gasoline',
    mileage: 'Unlimited',
    available: true,
  },
  {
    id: 2,
    brand: 'Honda',
    model: 'Civic',
    type: 'Sedan',
    price: 40,
    features: 'Manual, 4 Seats, Bluetooth, Air Conditioning',
    seats: 4,
    transmission: 'Manual',
    fuelType: 'Gasoline',
    mileage: 'Unlimited',
    available: true,
  },
  {
    id: 3,
    brand: 'BMW',
    model: 'X5',
    type: 'SUV',
    price: 120,
    features: 'Automatic, 7 Seats, GPS, Leather, Sunroof',
    seats: 7,
    transmission: 'Automatic',
    fuelType: 'Gasoline',
    mileage: 'Unlimited',
    available: true,
  },
  {
    id: 4,
    brand: 'Mercedes',
    model: 'C-Class',
    type: 'Luxury',
    price: 150,
    features: 'Automatic, 4 Seats, Premium Sound, Leather, Navigation',
    seats: 4,
    transmission: 'Automatic',
    fuelType: 'Gasoline',
    mileage: 'Unlimited',
    available: true,
  },
  {
    id: 5,
    brand: 'Audi',
    model: 'A4',
    type: 'Luxury',
    price: 130,
    features: 'Automatic, 4 Seats, Quattro AWD, Premium Interior',
    seats: 4,
    transmission: 'Automatic',
    fuelType: 'Gasoline',
    mileage: 'Unlimited',
    available: true,
  },
  {
    id: 6,
    brand: 'Toyota',
    model: 'RAV4',
    type: 'SUV',
    price: 65,
    features: 'Automatic, 5 Seats, All-Wheel Drive, Cargo Space',
    seats: 5,
    transmission: 'Automatic',
    fuelType: 'Gasoline',
    mileage: 'Unlimited',
    available: true,
  },
  {
    id: 7,
    brand: 'Honda',
    model: 'Accord',
    type: 'Sedan',
    price: 50,
    features: 'Automatic, 4 Seats, Hybrid, Fuel Efficient',
    seats: 4,
    transmission: 'Automatic',
    fuelType: 'Hybrid',
    mileage: 'Unlimited',
    available: true,
  },
  {
    id: 8,
    brand: 'BMW',
    model: '3 Series',
    type: 'Luxury',
    price: 110,
    features: 'Automatic, 4 Seats, Sport Package, Premium Sound',
    seats: 4,
    transmission: 'Automatic',
    fuelType: 'Gasoline',
    mileage: 'Unlimited',
    available: true,
  },
]

const rentalDays = computed(() => {
  if (!rentalData.value.pickupDate || !rentalData.value.returnDate) return 0
  const pickup = new Date(rentalData.value.pickupDate)
  const returnDate = new Date(rentalData.value.returnDate)
  const diffTime = returnDate - pickup
  const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24))
  return diffDays > 0 ? diffDays : 0
})

const totalPrice = computed(() => {
  return car.value ? rentalDays.value * car.value.price : 0
})

const canProceed = computed(() => {
  return rentalData.value.pickupDate && rentalData.value.returnDate && rentalDays.value > 0
})

const proceedToRental = () => {
  if (canProceed.value) {
    router.push({
      name: 'rental',
      params: { id: car.value.id },
      query: {
        pickupDate: rentalData.value.pickupDate,
        returnDate: rentalData.value.returnDate,
        totalPrice: totalPrice.value,
      },
    })
  }
}

onMounted(() => {
  const carId = parseInt(route.params.id)
  car.value = cars.find((c) => c.id === carId)

  if (!car.value) {
    router.push('/cars')
  }
})
</script>

<style scoped>
.car-detail-page {
  margin-top: 70px;
  padding: 40px 20px;
  min-height: 100vh;
  background: #f8fafc;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
}

.car-detail {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 3rem;
  background: white;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.car-images {
  padding: 2rem;
}

.main-image {
  height: 300px;
  background: #f3f4f6;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.car-placeholder-large {
  font-size: 6rem;
  opacity: 0.7;
}

.car-info {
  padding: 2rem;
}

.car-info h1 {
  font-size: 2rem;
  color: #1f2937;
  margin-bottom: 0.5rem;
}

.car-type {
  color: #6b7280;
  margin-bottom: 1rem;
}

.car-price {
  font-size: 1.8rem;
  font-weight: bold;
  color: #059669;
  margin-bottom: 2rem;
}

.car-features,
.car-specs {
  margin-bottom: 2rem;
}

.car-features h3,
.car-specs h3 {
  font-size: 1.2rem;
  color: #1f2937;
  margin-bottom: 1rem;
}

.car-features ul {
  list-style: none;
  padding: 0;
}

.car-features li {
  padding: 0.5rem 0;
  border-bottom: 1px solid #e5e7eb;
}

.car-features li:last-child {
  border-bottom: none;
}

.specs-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.spec {
  display: flex;
  justify-content: space-between;
  padding: 0.5rem 0;
  border-bottom: 1px solid #e5e7eb;
}

.spec-label {
  color: #6b7280;
}

.spec-value {
  font-weight: 500;
  color: #1f2937;
}

.rental-section {
  background: #f8fafc;
  padding: 2rem;
  border-radius: 10px;
  margin-top: 2rem;
}

.rental-section h3 {
  font-size: 1.2rem;
  color: #1f2937;
  margin-bottom: 1.5rem;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  margin-bottom: 1.5rem;
}

.form-group {
  display: flex;
  flex-direction: column;
}

.form-group label {
  margin-bottom: 0.5rem;
  color: #374151;
  font-weight: 500;
}

.form-group input {
  padding: 8px 12px;
  border: 1px solid #d1d5db;
  border-radius: 6px;
  font-size: 14px;
}

.rental-summary {
  background: white;
  padding: 1.5rem;
  border-radius: 6px;
  margin-bottom: 1.5rem;
}

.summary-row {
  display: flex;
  justify-content: space-between;
  padding: 0.5rem 0;
  border-bottom: 1px solid #e5e7eb;
}

.summary-row:last-child {
  border-bottom: none;
}

.summary-row.total {
  font-weight: bold;
  font-size: 1.1rem;
  color: #059669;
}

.btn {
  padding: 12px 24px;
  border: none;
  border-radius: 6px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
}

.btn-primary {
  background: #2563eb;
  color: white;
}

.btn-primary:hover:not(:disabled) {
  background: #1d4ed8;
}

.btn-primary:disabled {
  background: #9ca3af;
  cursor: not-allowed;
}

.btn-large {
  width: 100%;
  padding: 16px;
  font-size: 1.1rem;
}

.loading {
  text-align: center;
  padding: 4rem;
  color: #6b7280;
}

@media (max-width: 768px) {
  .car-detail {
    grid-template-columns: 1fr;
    gap: 0;
  }

  .form-row {
    grid-template-columns: 1fr;
  }

  .specs-grid {
    grid-template-columns: 1fr;
  }
}
</style>
