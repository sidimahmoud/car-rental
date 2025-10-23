<template>
  <div class="rental-page">
    <div class="container">
      <div v-if="car" class="rental-container">
        <div class="rental-header">
          <h1>Complete Your Rental</h1>
          <p>Review your booking details and complete the rental process</p>
        </div>

        <div class="rental-content">
          <div class="car-summary">
            <h2>Car Details</h2>
            <div class="car-info-card">
              <div class="car-image">
                <div class="car-placeholder">🚗</div>
              </div>
              <div class="car-details">
                <h3>{{ car.brand }} {{ car.model }}</h3>
                <p class="car-type">{{ car.type }}</p>
                <p class="car-features">{{ car.features }}</p>
              </div>
            </div>
          </div>

          <div class="rental-details">
            <h2>Rental Information</h2>
            <div class="details-card">
              <div class="detail-row">
                <span class="label">Pickup Date:</span>
                <span class="value">{{ formatDate(rentalData.pickupDate) }}</span>
              </div>
              <div class="detail-row">
                <span class="label">Return Date:</span>
                <span class="value">{{ formatDate(rentalData.returnDate) }}</span>
              </div>
              <div class="detail-row">
                <span class="label">Duration:</span>
                <span class="value">{{ rentalDays }} days</span>
              </div>
              <div class="detail-row">
                <span class="label">Daily Rate:</span>
                <span class="value">${{ car.price }}</span>
              </div>
              <div class="detail-row total">
                <span class="label">Total Amount:</span>
                <span class="value">${{ totalPrice }}</span>
              </div>
            </div>
          </div>

          <div class="customer-info">
            <h2>Your Information</h2>
            <form @submit.prevent="completeRental" class="customer-form">
              <div class="form-row">
                <div class="form-group">
                  <label for="firstName">First Name</label>
                  <input type="text" id="firstName" v-model="customerInfo.firstName" required />
                </div>
                <div class="form-group">
                  <label for="lastName">Last Name</label>
                  <input type="text" id="lastName" v-model="customerInfo.lastName" required />
                </div>
              </div>

              <div class="form-group">
                <label for="email">Email</label>
                <input type="email" id="email" v-model="customerInfo.email" required />
              </div>

              <div class="form-group">
                <label for="phone">Phone Number</label>
                <input type="tel" id="phone" v-model="customerInfo.phone" required />
              </div>

              <div class="form-group">
                <label for="license">Driver's License Number</label>
                <input type="text" id="license" v-model="customerInfo.license" required />
              </div>

              <div class="form-group">
                <label for="address">Address</label>
                <textarea id="address" v-model="customerInfo.address" rows="3" required></textarea>
              </div>

              <div class="terms">
                <label class="checkbox-label">
                  <input type="checkbox" v-model="customerInfo.agreeTerms" required />
                  <span class="checkmark"></span>
                  I agree to the <a href="#" @click.prevent>Terms and Conditions</a>
                </label>
              </div>

              <button
                type="submit"
                class="btn btn-primary btn-large"
                :disabled="!canCompleteRental || loading"
              >
                {{ loading ? 'Processing...' : 'Complete Rental' }}
              </button>
            </form>
          </div>
        </div>

        <div v-if="error" class="error-message">
          {{ error }}
        </div>

        <div v-if="success" class="success-message">
          <h3>🎉 Rental Confirmed!</h3>
          <p>
            Your rental has been successfully booked. You will receive a confirmation email shortly.
          </p>
          <RouterLink to="/" class="btn btn-primary">Return to Home</RouterLink>
        </div>
      </div>

      <div v-else class="loading">
        <p>Loading rental details...</p>
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
const loading = ref(false)
const success = ref(false)
const error = ref('')

const rentalData = ref({
  pickupDate: '',
  returnDate: '',
})

const customerInfo = ref({
  firstName: '',
  lastName: '',
  email: '',
  phone: '',
  license: '',
  address: '',
  agreeTerms: false,
})

const cars = [
  {
    id: 1,
    brand: 'Toyota',
    model: 'Camry',
    type: 'Sedan',
    price: 45,
    features: 'Automatic, 4 Seats, Air Conditioning, Bluetooth, GPS',
    available: true,
  },
  {
    id: 2,
    brand: 'Honda',
    model: 'Civic',
    type: 'Sedan',
    price: 40,
    features: 'Manual, 4 Seats, Bluetooth, Air Conditioning',
    available: true,
  },
  {
    id: 3,
    brand: 'BMW',
    model: 'X5',
    type: 'SUV',
    price: 120,
    features: 'Automatic, 7 Seats, GPS, Leather, Sunroof',
    available: true,
  },
  {
    id: 4,
    brand: 'Mercedes',
    model: 'C-Class',
    type: 'Luxury',
    price: 150,
    features: 'Automatic, 4 Seats, Premium Sound, Leather, Navigation',
    available: true,
  },
  {
    id: 5,
    brand: 'Audi',
    model: 'A4',
    type: 'Luxury',
    price: 130,
    features: 'Automatic, 4 Seats, Quattro AWD, Premium Interior',
    available: true,
  },
  {
    id: 6,
    brand: 'Toyota',
    model: 'RAV4',
    type: 'SUV',
    price: 65,
    features: 'Automatic, 5 Seats, All-Wheel Drive, Cargo Space',
    available: true,
  },
  {
    id: 7,
    brand: 'Honda',
    model: 'Accord',
    type: 'Sedan',
    price: 50,
    features: 'Automatic, 4 Seats, Hybrid, Fuel Efficient',
    available: true,
  },
  {
    id: 8,
    brand: 'BMW',
    model: '3 Series',
    type: 'Luxury',
    price: 110,
    features: 'Automatic, 4 Seats, Sport Package, Premium Sound',
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

const canCompleteRental = computed(() => {
  return (
    customerInfo.value.firstName &&
    customerInfo.value.lastName &&
    customerInfo.value.email &&
    customerInfo.value.phone &&
    customerInfo.value.license &&
    customerInfo.value.address &&
    customerInfo.value.agreeTerms
  )
})

const formatDate = (dateString) => {
  return new Date(dateString).toLocaleDateString('en-US', {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
  })
}

const completeRental = async () => {
  loading.value = true
  error.value = ''

  try {
    // Simulate API call
    await new Promise((resolve) => setTimeout(resolve, 2000))

    // Here you would typically save to Supabase
    console.log('Rental completed:', {
      car: car.value,
      rentalData: rentalData.value,
      customerInfo: customerInfo.value,
      totalPrice: totalPrice.value,
    })

    success.value = true
  } catch (err) {
    error.value = 'Failed to complete rental. Please try again.'
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  const carId = parseInt(route.params.id)
  car.value = cars.find((c) => c.id === carId)

  if (!car.value) {
    router.push('/cars')
    return
  }

  // Get rental data from query params
  rentalData.value.pickupDate = route.query.pickupDate || ''
  rentalData.value.returnDate = route.query.returnDate || ''
})
</script>

<style scoped>
.rental-page {
  margin-top: 70px;
  padding: 40px 20px;
  min-height: 100vh;
  background: #f8fafc;
}

.container {
  max-width: 1000px;
  margin: 0 auto;
}

.rental-container {
  background: white;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.rental-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 2rem;
  text-align: center;
}

.rental-header h1 {
  font-size: 2rem;
  margin-bottom: 0.5rem;
}

.rental-header p {
  opacity: 0.9;
}

.rental-content {
  padding: 2rem;
}

.car-summary,
.rental-details,
.customer-info {
  margin-bottom: 3rem;
}

.car-summary h2,
.rental-details h2,
.customer-info h2 {
  font-size: 1.5rem;
  color: #1f2937;
  margin-bottom: 1rem;
}

.car-info-card {
  display: flex;
  background: #f8fafc;
  border-radius: 8px;
  padding: 1.5rem;
  gap: 1.5rem;
}

.car-image {
  width: 100px;
  height: 80px;
  background: #e5e7eb;
  border-radius: 6px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.car-placeholder {
  font-size: 2rem;
  opacity: 0.7;
}

.car-details h3 {
  font-size: 1.2rem;
  color: #1f2937;
  margin-bottom: 0.5rem;
}

.car-type {
  color: #6b7280;
  margin-bottom: 0.5rem;
}

.car-features {
  color: #6b7280;
  font-size: 0.9rem;
}

.details-card {
  background: #f8fafc;
  border-radius: 8px;
  padding: 1.5rem;
}

.detail-row {
  display: flex;
  justify-content: space-between;
  padding: 0.75rem 0;
  border-bottom: 1px solid #e5e7eb;
}

.detail-row:last-child {
  border-bottom: none;
}

.detail-row.total {
  font-weight: bold;
  font-size: 1.1rem;
  color: #059669;
}

.label {
  color: #6b7280;
}

.value {
  color: #1f2937;
  font-weight: 500;
}

.customer-form {
  background: #f8fafc;
  border-radius: 8px;
  padding: 1.5rem;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  margin-bottom: 1rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  margin-bottom: 1rem;
}

.form-group label {
  margin-bottom: 0.5rem;
  color: #374151;
  font-weight: 500;
}

.form-group input,
.form-group textarea {
  padding: 10px 12px;
  border: 1px solid #d1d5db;
  border-radius: 6px;
  font-size: 14px;
  transition: border-color 0.3s;
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #2563eb;
}

.terms {
  margin: 1.5rem 0;
}

.checkbox-label {
  display: flex;
  align-items: center;
  cursor: pointer;
  color: #374151;
}

.checkbox-label input[type='checkbox'] {
  margin-right: 0.5rem;
}

.checkbox-label a {
  color: #2563eb;
  text-decoration: none;
}

.checkbox-label a:hover {
  text-decoration: underline;
}

.btn {
  padding: 12px 24px;
  border: none;
  border-radius: 6px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
  text-decoration: none;
  display: inline-block;
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

.error-message {
  background: #fef2f2;
  color: #dc2626;
  padding: 12px;
  border-radius: 6px;
  margin: 1rem 0;
  text-align: center;
}

.success-message {
  background: #f0fdf4;
  color: #166534;
  padding: 2rem;
  border-radius: 8px;
  text-align: center;
  margin-top: 2rem;
}

.success-message h3 {
  font-size: 1.5rem;
  margin-bottom: 1rem;
}

.loading {
  text-align: center;
  padding: 4rem;
  color: #6b7280;
}

@media (max-width: 768px) {
  .form-row {
    grid-template-columns: 1fr;
  }

  .car-info-card {
    flex-direction: column;
  }

  .car-image {
    width: 100%;
    height: 120px;
  }
}
</style>
