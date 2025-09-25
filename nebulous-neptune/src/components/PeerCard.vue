<template>
  <div class="peer-card">
    <!-- Header -->
    <div class="peer-header">
      <div class="peer-avatar initials">{{ initials }}</div>
      <div>
        <strong>{{ name }}</strong>
        <div class="peer-status">Logged in {{ loggedIn }}</div>
      </div>
    </div>

    <!-- About Me -->
    <div class="peer-about">
      <h4>ABOUT ME</h4>
      <p>{{ about }}</p>
    </div>

    <!-- Details with icons -->
    <ul class="peer-details">
      <li>
        <img :src="AvailabilityIcon" alt="availability" />
        Available on {{ available }}
      </li>
      <li>
        <img :src="TargetingIcon" alt="targeting" />
        Targeting {{ companies }}
      </li>
      <li>
        <img :src="SessionsIcon" alt="sessions" />
        {{ sessions }} sessions booked
      </li>
      <li>
        <img :src="ExperienceIcon" alt="experience" />
        {{ experience }} years of experience
      </li>
      <li>
        <img :src="LocationIcon" alt="location" />
        {{ location }}
      </li>
    </ul>

    <!-- Footer -->
    <div class="peer-footer">BOOK ME</div>
  </div>
</template>

<script setup>
import { computed } from "vue";

// Import SVGs from assets folder
import AvailabilityIcon from "../assets/Availability.svg?url";
import TargetingIcon    from "../assets/Targeting.svg?url";
import SessionsIcon     from "../assets/Sessions.svg?url";
import ExperienceIcon   from "../assets/Experience.svg?url";
import LocationIcon     from "../assets/Location.svg?url";

const props = defineProps({
  name: String,
  loggedIn: String,
  about: {
    type: String,
    default:
      "I am a seasoned product manager with extensive experience in interview preparation.",
  },
  available: String,
  companies: String,
  sessions: Number,
  experience: Number,
  location: String,
});

// Show initials if no avatar provided
const initials = computed(() => {
  if (!props.name) return "";
  return props.name
    .split(" ")
    .map((n) => n[0])
    .join("")
    .slice(0, 2)
    .toUpperCase();
});
</script>

<style scoped>
.peer-card {
  background: #fff;
  border: 1px solid #e5e7eb;
  border-radius: 6px;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.peer-header {
  display: flex;
  align-items: center;
  gap: 0.8rem;
}

.peer-avatar {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background: #ddd;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  font-size: 0.9rem;
  color: #555;
}

.peer-status {
  font-size: 0.85rem;
  color: #555;
}

.peer-about h4 {
  text-transform: uppercase;
  font-size: 0.75rem;
  color: #666;
  margin: 0 0 0.3rem 0;
}

.peer-about p {
  margin: 0;
  font-size: 0.9rem;
  line-height: 1.4;
  color: #333;
}

/* 👇 This is the styles I mentioned */
.peer-details {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
  font-size: 0.9rem;
  color: #444;
}

.peer-details li {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.peer-details img {
  width: 16px;
  height: 16px;
}

.peer-footer {
  text-align: center;
  font-weight: 600;
  color: #0070f3;
  cursor: pointer;
  margin-top: auto;
}
</style>
