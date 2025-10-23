<template>
  <div class="home">
    <!-- Hero Section -->
    <section class="hero">
      <div class="hero-background">
        <div class="hero-shapes">
          <div class="shape shape-1"></div>
          <div class="shape shape-2"></div>
          <div class="shape shape-3"></div>
        </div>
      </div>
      <div class="hero-content">
        <div class="hero-text">
          <h1 class="hero-title">
            <span class="gradient-text">Premium</span> Car Rentals
            <br />Made Simple
          </h1>
          <p class="hero-subtitle">
            Experience luxury and comfort with our premium fleet. 
            From sports cars to SUVs, find your perfect ride in seconds.
          </p>
          <div class="hero-buttons">
            <RouterLink to="/cars" class="btn btn-primary">
              <span>Explore Fleet</span>
              <svg class="btn-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7l5 5m0 0l-5 5m5-5H6"/>
              </svg>
            </RouterLink>
            <RouterLink to="/about" class="btn btn-secondary">
              <span>Learn More</span>
            </RouterLink>
          </div>
          <div class="hero-stats">
            <div class="stat">
              <div class="stat-number">500+</div>
              <div class="stat-label">Happy Customers</div>
            </div>
            <div class="stat">
              <div class="stat-number">50+</div>
              <div class="stat-label">Premium Cars</div>
            </div>
            <div class="stat">
              <div class="stat-number">24/7</div>
              <div class="stat-label">Support</div>
            </div>
          </div>
        </div>
        <div class="hero-visual">
          <div class="car-showcase">
            <div class="car-card-hero">
              <div class="car-image-hero">
                <div class="car-3d">🚗</div>
                <div class="car-glow"></div>
              </div>
              <div class="car-info-hero">
                <h3>BMW X5</h3>
                <p>Luxury SUV</p>
                <div class="price-tag">$120/day</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Features Section -->
    <section class="features">
      <div class="container">
        <div class="section-header">
          <h2 class="section-title">Why Choose <span class="gradient-text">CarRent</span>?</h2>
          <p class="section-subtitle">Experience the difference with our premium service</p>
        </div>
        <div class="features-grid">
          <div class="feature-card" v-for="(feature, index) in features" :key="index">
            <div class="feature-image">
              <div class="feature-placeholder">{{ feature.icon }}</div>
            </div>
            <div class="feature-info">
              <div class="feature-header">
                <h3 class="feature-name">{{ feature.title }}</h3>
              </div>
              <p class="feature-description">{{ feature.description }}</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Popular Cars Section -->
    <section class="popular-cars">
      <div class="container">
        <div class="section-header">
          <h2 class="section-title">Our <span class="gradient-text">Premium</span> Fleet</h2>
          <p class="section-subtitle">Choose from our carefully curated collection of luxury vehicles</p>
        </div>
        <div class="cars-grid">
          <div class="car-card" v-for="car in popularCars" :key="car.id" @click="viewCar(car.id)">
            <div class="car-image">
              <div class="car-placeholder">{{ car.emoji }}</div>
              <div class="car-overlay">
                <div class="car-badge">{{ car.badge }}</div>
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
                <button class="btn btn-outline">View Details</button>
              </div>
            </div>
          </div>
        </div>
        <div class="text-center">
          <RouterLink to="/cars" class="btn btn-primary btn-large">
            <span>View All Cars</span>
            <svg class="btn-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7l5 5m0 0l-5 5m5-5H6"/>
            </svg>
          </RouterLink>
        </div>
      </div>
    </section>

    <!-- CTA Section -->
    <section class="cta-section">
      <div class="container">
        <div class="cta-content">
          <h2>Ready to Start Your Journey?</h2>
          <p>Join thousands of satisfied customers who trust us for their transportation needs</p>
          <RouterLink to="/cars" class="btn btn-primary btn-large">
            <span>Book Your Car Now</span>
            <svg class="btn-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7l5 5m0 0l-5 5m5-5H6"/>
            </svg>
          </RouterLink>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { RouterLink, useRouter } from 'vue-router'

const router = useRouter()

const features = ref([
  {
    icon: '⚡',
    title: 'Instant Booking',
    description: 'Book your car in under 60 seconds with our streamlined process'
  },
  {
    icon: '🛡️',
    title: 'Fully Insured',
    description: 'All vehicles are comprehensively insured for your peace of mind'
  },
  {
    icon: '💰',
    title: 'Best Prices',
    description: 'Competitive rates with transparent pricing and no hidden fees'
  },
  {
    icon: '🚗',
    title: 'Premium Fleet',
    description: 'Carefully selected luxury vehicles from top manufacturers'
  },
])

const popularCars = ref([
  { 
    id: 1, 
    brand: 'BMW', 
    model: 'X5', 
    type: 'Luxury SUV', 
    price: 120,
    emoji: '🚗',
    badge: 'Popular',
    features: 'Automatic, 7 Seats, GPS, Leather, Sunroof',
    seats: 7,
    transmission: 'Automatic',
    fuelType: 'Gasoline',
    mileage: 'Unlimited',
    available: true
  },
  { 
    id: 2, 
    brand: 'Mercedes', 
    model: 'C-Class', 
    type: 'Luxury Sedan', 
    price: 150,
    emoji: '🚙',
    badge: 'Premium',
    features: 'Automatic, 4 Seats, Premium Sound, Leather, Navigation',
    seats: 4,
    transmission: 'Automatic',
    fuelType: 'Gasoline',
    mileage: 'Unlimited',
    available: true
  },
  { 
    id: 3, 
    brand: 'Audi', 
    model: 'A4', 
    type: 'Luxury Sedan', 
    price: 130,
    emoji: '🚘',
    badge: 'New',
    features: 'Automatic, 4 Seats, Quattro AWD, Premium Interior',
    seats: 4,
    transmission: 'Automatic',
    fuelType: 'Gasoline',
    mileage: 'Unlimited',
    available: true
  },
  { 
    id: 4, 
    brand: 'Tesla', 
    model: 'Model 3', 
    type: 'Electric', 
    price: 180,
    emoji: '⚡',
    badge: 'Eco',
    features: 'Automatic, 4 Seats, Electric, Autopilot, Fast Charging',
    seats: 4,
    transmission: 'Automatic',
    fuelType: 'Electric',
    mileage: 'Unlimited',
    available: true
  }
])

const viewCar = (carId) => {
  router.push(`/car/${carId}`)
}
</script>

<style scoped>
/* Global Styles */
.home {
  margin-top: 0;
  overflow-x: hidden;
  width: 100vw;
  max-width: 100vw;
}

.container {
  max-width: 100%;
  margin: 0 auto;
  padding: 0 2rem;
  width: 100%;
}

.gradient-text {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* Hero Section */
.hero {
  position: relative;
  min-height: 100vh;
  display: flex;
  align-items: center;
  background: linear-gradient(135deg, #0f0f23 0%, #1a1a2e 50%, #16213e 100%);
  color: white;
  overflow: hidden;
  width: 100vw;
  max-width: 100vw;
}

.hero-background {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 1;
}

.hero-shapes {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.shape {
  position: absolute;
  border-radius: 50%;
  background: linear-gradient(45deg, rgba(102, 126, 234, 0.1), rgba(118, 75, 162, 0.1));
  animation: float 6s ease-in-out infinite;
}

.shape-1 {
  width: 300px;
  height: 300px;
  top: 10%;
  right: 10%;
  animation-delay: 0s;
}

.shape-2 {
  width: 200px;
  height: 200px;
  top: 60%;
  left: 5%;
  animation-delay: 2s;
}

.shape-3 {
  width: 150px;
  height: 150px;
  top: 30%;
  left: 60%;
  animation-delay: 4s;
}

@keyframes float {
  0%, 100% { transform: translateY(0px) rotate(0deg); }
  50% { transform: translateY(-20px) rotate(180deg); }
}

.hero-content {
  position: relative;
  z-index: 2;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  align-items: center;
  width: 100vw;
  max-width: 100vw;
  margin: 0;
  padding: 0 2rem;
  min-height: 100vh;
}

.hero-title {
  font-size: 4rem;
  font-weight: 800;
  line-height: 1.1;
  margin-bottom: 1.5rem;
  letter-spacing: -0.02em;
}

.hero-subtitle {
  font-size: 1.25rem;
  line-height: 1.6;
  margin-bottom: 2.5rem;
  opacity: 0.9;
  max-width: 500px;
}

.hero-buttons {
  display: flex;
  gap: 1rem;
  margin-bottom: 3rem;
}

.btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 1rem 2rem;
  border-radius: 12px;
  text-decoration: none;
  font-weight: 600;
  font-size: 1rem;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
}

.btn-primary {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  box-shadow: 0 10px 25px rgba(102, 126, 234, 0.3);
}

.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 20px 40px rgba(102, 126, 234, 0.4);
}

.btn-secondary {
  background: rgba(255, 255, 255, 0.1);
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(10px);
}

.btn-secondary:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: translateY(-2px);
}

.btn-icon {
  width: 20px;
  height: 20px;
  transition: transform 0.3s;
}

.btn:hover .btn-icon {
  transform: translateX(4px);
}

.hero-stats {
  display: flex;
  gap: 2rem;
}

.stat {
  text-align: center;
}

.stat-number {
  font-size: 2rem;
  font-weight: 800;
  color: #667eea;
  margin-bottom: 0.25rem;
}

.stat-label {
  font-size: 0.875rem;
  opacity: 0.8;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

/* Hero Visual */
.hero-visual {
  display: flex;
  justify-content: center;
  align-items: center;
}

.car-showcase {
  position: relative;
  margin-left: auto;
  width: 60%;
}

.car-card-hero {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 20px;
  padding: 2rem;
  text-align: center;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.3);
}

.car-image-hero {
  position: relative;
  margin-bottom: 1.5rem;
}

.car-3d {
  font-size: 6rem;
  filter: drop-shadow(0 10px 20px rgba(0, 0, 0, 0.3));
  animation: bounce 2s ease-in-out infinite;
}

.car-glow {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 120px;
  height: 120px;
  background: radial-gradient(circle, rgba(102, 126, 234, 0.3) 0%, transparent 70%);
  border-radius: 50%;
  animation: pulse 2s ease-in-out infinite;
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

@keyframes pulse {
  0%, 100% { opacity: 0.5; transform: translate(-50%, -50%) scale(1); }
  50% { opacity: 0.8; transform: translate(-50%, -50%) scale(1.1); }
}

.car-info-hero h3 {
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 0.5rem;
}

.car-info-hero p {
  opacity: 0.8;
  margin-bottom: 1rem;
}

.price-tag {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 20px;
  font-weight: 600;
  display: inline-block;
}

/* Features Section */
.features {
  padding: 6rem 0;
  background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 100%);
}

.section-header {
  text-align: center;
  margin-bottom: 4rem;
}

.section-title {
  font-size: 3rem;
  font-weight: 800;
  margin-bottom: 1rem;
  color: #1a202c;
}

.section-subtitle {
  font-size: 1.25rem;
  color: #64748b;
  max-width: 600px;
  margin: 0 auto;
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 2rem;
  margin-bottom: 3rem;
}

.feature-card {
  background: white;
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  cursor: pointer;
  border: 1px solid rgba(0, 0, 0, 0.05);
}

.feature-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.15);
}

.feature-image {
  position: relative;
  height: 200px;
  background: linear-gradient(135deg, #f1f5f9 0%, #e2e8f0 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.feature-placeholder {
  font-size: 4rem;
  filter: drop-shadow(0 5px 15px rgba(0, 0, 0, 0.1));
}

.feature-overlay {
  position: absolute;
  top: 1rem;
  right: 1rem;
}

.feature-badge {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 0.25rem 0.75rem;
  border-radius: 20px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.feature-info {
  padding: 1.5rem;
}

.feature-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 0.5rem;
}

.feature-name {
  font-size: 1.25rem;
  font-weight: 700;
  color: #1a202c;
  margin: 0;
}

.feature-rating {
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

.feature-type {
  color: #64748b;
  margin-bottom: 1rem;
  font-size: 0.875rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.feature-features-list {
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

.feature-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.feature-benefit {
  display: flex;
  align-items: baseline;
  gap: 0.25rem;
}

.benefit-text {
  font-size: 1.5rem;
  font-weight: 800;
  color: #059669;
}

.btn-outline {
  background: transparent;
  color: #667eea;
  border: 2px solid #667eea;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  font-weight: 600;
  font-size: 0.875rem;
  transition: all 0.3s;
}

.btn-outline:hover {
  background: #667eea;
  color: white;
}

/* Popular Cars Section */
.popular-cars {
  padding: 6rem 0;
  background: white;
}

.cars-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 2rem;
  margin-bottom: 3rem;
}

.car-card {
  background: white;
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  cursor: pointer;
  border: 1px solid rgba(0, 0, 0, 0.05);
}

.car-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.15);
}

.car-image {
  position: relative;
  height: 200px;
  background: linear-gradient(135deg, #f1f5f9 0%, #e2e8f0 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.car-placeholder {
  font-size: 4rem;
  filter: drop-shadow(0 5px 15px rgba(0, 0, 0, 0.1));
}

.car-overlay {
  position: absolute;
  top: 1rem;
  right: 1rem;
}

.car-badge {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 0.25rem 0.75rem;
  border-radius: 20px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
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

.btn-outline {
  background: transparent;
  color: #667eea;
  border: 2px solid #667eea;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  font-weight: 600;
  font-size: 0.875rem;
  transition: all 0.3s;
}

.btn-outline:hover {
  background: #667eea;
  color: white;
}

.btn-large {
  padding: 1rem 2rem;
  font-size: 1.125rem;
}

.text-center {
  text-align: center;
}

/* CTA Section */
.cta-section {
  padding: 6rem 0;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  text-align: center;
}

.cta-content h2 {
  font-size: 3rem;
  font-weight: 800;
  margin-bottom: 1rem;
}

.cta-content p {
  font-size: 1.25rem;
  margin-bottom: 2rem;
  opacity: 0.9;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
}

/* Responsive Design */
@media (max-width: 1024px) {
  .hero-content {
    grid-template-columns: 1fr;
    gap: 3rem;
    text-align: center;
  }
  
  .hero-title {
    font-size: 3rem;
  }
  
  .features-grid {
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1.5rem;
  }
}

@media (max-width: 768px) {
  .container {
    padding: 0 1rem;
  }
  
  .hero-title {
    font-size: 2.5rem;
  }
  
  .hero-buttons {
    flex-direction: column;
    align-items: center;
  }
  
  .hero-stats {
    justify-content: center;
  }
  
  .section-title {
    font-size: 2rem;
  }
  
  .features-grid {
    grid-template-columns: 1fr;
  }
  
  .cars-grid {
    grid-template-columns: 1fr;
  }
  
  .cta-content h2 {
    font-size: 2rem;
  }
}
</style>
