<template>
  <div class="about-page">
    <div class="container">
      <div class="about-header">
        <h1>About CarRent</h1>
        <p>Your trusted partner for car rentals</p>
      </div>

      <div class="about-content">
        <div class="about-section">
          <h2>Our Mission</h2>
          <p>
            At CarRent, we believe that everyone deserves access to reliable, affordable
            transportation. Our mission is to provide a seamless car rental experience that connects
            you with the perfect vehicle for your journey, whether it's a quick city trip or a long
            road adventure.
          </p>
        </div>

        <div class="about-section">
          <h2>Why Choose Us?</h2>
          <div class="features-grid">
            <div class="feature">
              <div class="feature-icon">🚗</div>
              <h3>Wide Selection</h3>
              <p>
                From economy cars to luxury vehicles, we have something for every need and budget.
              </p>
            </div>
            <div class="feature">
              <div class="feature-icon">⚡</div>
              <h3>Quick Booking</h3>
              <p>Book your car in minutes with our streamlined online process.</p>
            </div>
            <div class="feature">
              <div class="feature-icon">🛡️</div>
              <h3>Safe & Secure</h3>
              <p>
                All our vehicles are regularly inspected and fully insured for your peace of mind.
              </p>
            </div>
            <div class="feature">
              <div class="feature-icon">💰</div>
              <h3>Best Prices</h3>
              <p>Competitive rates with transparent pricing and no hidden fees.</p>
            </div>
          </div>
        </div>

        <div class="about-section">
          <h2>Our Story</h2>
          <p>
            Founded in 2024, CarRent started with a simple vision: to make car rental accessible,
            affordable, and hassle-free for everyone. We've grown from a small local service to a
            trusted platform that serves customers across the region.
          </p>
          <p>
            Our team is passionate about mobility and committed to providing exceptional service. We
            understand that every journey is unique, and we're here to help you find the perfect car
            for your adventure.
          </p>
        </div>

        <div class="contact-section">
          <h2>Get in Touch</h2>
          <div class="contact-info">
            <div class="contact-item"><strong>Email:</strong> sidmah10@gmail.com</div>
            <div class="contact-item"><strong>Phone:</strong> (222) 33306060</div>
            <div class="contact-item">
              <strong>Address:</strong> Worldwide
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// No additional logic needed for this static page
</script>

<style scoped>
.about-page {
  margin-top: 70px;
  padding: 40px 20px;
  min-height: 100vh;
  background: #f8fafc;
}

.container {
  max-width: 1000px;
  margin: 0 auto;
}

.about-header {
  text-align: center;
  margin-bottom: 4rem;
}

.about-header h1 {
  font-size: 3rem;
  color: #1f2937;
  margin-bottom: 1rem;
}

.about-header p {
  font-size: 1.2rem;
  color: #6b7280;
}

.about-content {
  background: white;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 3rem;
}

.about-section {
  margin-bottom: 3rem;
}

.about-section h2 {
  font-size: 2rem;
  color: #1f2937;
  margin-bottom: 1.5rem;
}

.about-section p {
  color: #6b7280;
  line-height: 1.8;
  margin-bottom: 1rem;
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
  margin-top: 2rem;
}

.feature {
  text-align: center;
  padding: 2rem;
  background: #f8fafc;
  border-radius: 10px;
}

.feature-icon {
  font-size: 3rem;
  margin-bottom: 1rem;
}

.feature h3 {
  font-size: 1.3rem;
  color: #1f2937;
  margin-bottom: 1rem;
}

.feature p {
  color: #6b7280;
  line-height: 1.6;
}

.contact-section {
  background: #f8fafc;
  padding: 2rem;
  border-radius: 10px;
  margin-top: 2rem;
}

.contact-section h2 {
  font-size: 1.8rem;
  color: #1f2937;
  margin-bottom: 1.5rem;
}

.contact-info {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.contact-item {
  color: #6b7280;
  font-size: 1.1rem;
}

.contact-item strong {
  color: #1f2937;
}

@media (max-width: 768px) {
  .about-header h1 {
    font-size: 2rem;
  }

  .about-content {
    padding: 2rem;
  }

  .features-grid {
    grid-template-columns: 1fr;
  }

  .contact-info {
    gap: 0.5rem;
  }
}
</style>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
