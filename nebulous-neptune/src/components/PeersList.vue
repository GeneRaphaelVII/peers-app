<script setup>
import { ref, computed, onMounted } from "vue";

// Import SVGs for the details
import AvailabilityIcon from "../assets/Availability.svg?url";
import TargetingIcon    from "../assets/Targeting.svg?url";
import SessionsIcon     from "../assets/Sessions.svg?url";
import ExperienceIcon   from "../assets/Experience.svg?url";
import LocationIcon     from "../assets/Location.svg?url";

// Reactive state
const peers = ref([]);
const selectedDays = ref([]);

// Days of the week
const daysOfWeek = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"];

// Helper: get day name from ISO date string
function getDayName(dateStr) {
  const date = new Date(dateStr);
  return date.toLocaleDateString("en-US", { weekday: "short" });
}

// Map availability to day-of-week
function mapPeerAvailability(peer) {
  const daysAvailable = new Set();
  peer.availability.forEach(slot => {
    const start = new Date(slot.start);
    const end = new Date(slot.end);
    const durationHours = (end - start) / (1000 * 60 * 60);
    if (durationHours >= 1) {
      daysAvailable.add(getDayName(slot.start));
    }
  });
  return { ...peer, daysAvailable: Array.from(daysAvailable) };
}

// Fetch peers JSON
onMounted(async () => {
  const res = await fetch("https://cdn.shopify.com/s/files/1/0417/7869/files/peers.json");
  const data = await res.json();
  peers.value = data.map(mapPeerAvailability);
});

// Filter peers based on selected days (OR)
const filteredPeers = computed(() => {
  if (selectedDays.value.length === 0) return peers.value;
  return peers.value.filter(peer =>
    peer.daysAvailable.some(day => selectedDays.value.includes(day))
  );
});

// Toggle selected day
function toggleDay(day) {
  if (selectedDays.value.includes(day)) {
    selectedDays.value = selectedDays.value.filter(d => d !== day);
  } else {
    selectedDays.value.push(day);
  }
}

// Compute initials for fallback avatars
function getInitials(name) {
  if (!name) return "";
  return name.split(" ").map(n => n[0]).join("").slice(0, 2).toUpperCase();
}
</script>

<template>
  <div>
    <!-- Header section -->
    <div class="header-blue">
      <div class="blue-bar"></div> <!-- just a blue rectangle -->

      <div class="Hi-Patrik-schedule">
        Hi Patrik, schedule your next practice session with a peer
      </div>

      <div class="FILTER-PEERS-AVAILAB">
        FILTER PEERS AVAILABLE ON:
      </div>

      <!-- Day filters -->
    <div class="day-filters">
      <button
        @click="selectedDays = []"
        :class="{ active: selectedDays.length === 0 }"
      >
        ALL
      </button>

      <button
        v-for="day in daysOfWeek"
        :key="day"
        @click="toggleDay(day)"
        :class="{ active: selectedDays.includes(day) }"
      >
        {{ day }}
      </button>
    </div>
    </div>

    <!-- Peer cards grid -->
    <div class="peer-list">
      <div v-for="peer in filteredPeers" :key="peer.name" class="peer-card">
        <!-- Header -->
        <div class="peer-header">
          <div class="peer-avatar Mask">
            <img v-if="peer.image" :src="peer.image" :alt="peer.name" />
            <span v-else>{{ getInitials(peer.name) }}</span>
          </div>
          <div>
            <strong>{{ peer.name }}</strong>
            <div class="peer-status">{{ peer.last_login }}</div>
          </div>
        </div>

        <!-- About -->
        <div class="peer-about">
          <h4>ABOUT ME</h4>
          <p>{{ peer.about_me }}</p>
        </div>

        <!-- Details -->
        <ul class="peer-details">
          <li>
            <img :src="AvailabilityIcon" alt="availability" />
            Available on {{ peer.daysAvailable.join(", ") }}
          </li>
          <li>
            <img :src="TargetingIcon" alt="targeting" />
            Targeting {{ peer.companies.join(", ") }}
          </li>
          <li>
            <img :src="SessionsIcon" alt="sessions" />
            {{ peer.sessions }} sessions booked
          </li>
          <li>
            <img :src="ExperienceIcon" alt="experience" />
            {{ peer.years_of_experience }} years of experience
          </li>
          <li>
            <img :src="LocationIcon" alt="location" />
            {{ peer.location }}
          </li>
        </ul>

        <!-- Footer -->
        <div class="peer-footer">BOOK ME</div>
      </div>
    </div>
  </div>
</template>

<style scoped>

.Hi-Patrik-schedule {
  width: 567px;
  height: 30px;
  margin: 0 0 16px 2px;
  font-family: "Open Sans", sans-serif;
  font-size: 20px;
  font-weight: bold;
  font-stretch: normal;
  font-style: normal;
  line-height: 1.5;
  letter-spacing: -0.1px;
  color: #333;
}

.FILTER-PEERS-AVAILAB {
  width: 172px;
  height: 20px;
  margin: 0 59px 10px 1px;
  font-family: "Open Sans", sans-serif;
  font-size: 12px;
  font-weight: bold;
  font-stretch: normal;
  font-style: normal;
  line-height: 1.67;
  letter-spacing: normal;
  color: #aaa;
}


/* Day filter buttons */
.day-filters {
  margin-bottom: 16px;
}
.day-filters button {
  margin: 0 4px;
  padding: 6px 12px;
  border: 1px solid #007bff;
  background: white;
  color: #007bff;
  cursor: pointer;
  border-radius: 0px;
}
.day-filters button.active {
  background: #007bff;
  color: white;
}

.peer-list {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
}

.peer-card {
  width: 268px;       /* match image width */
  min-height: 451px;  /* match image height */
  background: #fff;
  border: 1px solid #e5e7eb;
  border-radius: 6px;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.peer-avatar img {
  width: 268px;
  height: 451px;
  margin: 0 15px 16px 0;
  object-fit: contain;
}

/* Header */
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
  overflow: hidden;
}

.peer-avatar img {
  width: 268px;
  height: 451px;
  object-fit: contain;
  margin: 0 16px 16px 15px;
}

.peer-status {
  font-size: 0.85rem;
  color: #555;
}

/* About section */
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

/* Details list */
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

/* Footer */
.peer-footer {
  text-align: center;
  font-weight: 600;
  color: #0070f3;
  cursor: pointer;
  margin-top: auto;
}

.Mask {
  width: 72px;
  height: 72px;
  background-color: #d8d8d8;
  border-radius: 50%;        /* makes it circular */
  overflow: hidden;          /* crops the image inside the box */
  display: flex;
  align-items: center;
  justify-content: center;
}

.Mask img {
  width: 100%;
  height: 100%;
  object-fit: cover;         /* ensures image fills box and is cropped nicely */
}

.header-blue .day-filters {
  display: flex;       /* keep buttons in a row */
  flex-wrap: nowrap;   /* prevent wrapping to multiple lines */
  gap: 8px;            /* spacing between buttons */
}

.header-blue .day-filters button {
  background: white;
  color: #0070f3;
  border: 1px solid #0070f3;
  border-radius: 0px;
  padding: 6px 12px;
  cursor: pointer;
  font-weight: 500;
  white-space: nowrap; /* prevent text from wrapping inside the button */
}

.header-blue .day-filters button.active {
  background: #0055c8;
  color: white;
}

</style>
