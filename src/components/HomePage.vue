<template>
  <div class="container py-5 text-center">
    <h1 class="mb-3">🎓 University Dashboard</h1>
    <p class="text-muted mb-4">
      Responding Node: <span class="fw-bold text-primary">{{ frontendNode }}</span>
    </p>

    <div class="mb-4">
      <button @click="fetchStudents" class="btn btn-primary me-2">Students</button>
      <button @click="fetchSubjects" class="btn btn-secondary">Courses</button>
    </div>

    <div v-if="loading" class="alert alert-info">Loading data...</div>

    <div v-if="students.length > 0" class="mb-5">
      <h2 class="mb-4">👩‍🎓 Students List ({{ students.length }})</h2>
      <div class="row justify-content-center">
        <div v-for="(student, index) in students" :key="index" class="col-md-6 mb-3">
          <div class="card shadow-sm">
            <div class="card-body text-start">
              <h5 class="card-title mb-1">{{ student.name }}</h5>
              <p class="card-text text-muted">{{ student.program }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div v-if="Object.keys(subjects).length > 0">
      <h2 class="mb-4">📘 Courses List</h2>
      <div v-for="(subjectList, year) in subjects" :key="year" class="mb-4">
        <h4 class="text-start text-secondary">{{ year }}</h4>
        <div class="row justify-content-center">
          <div v-for="(subject, index) in subjectList" :key="index" class="col-md-6 mb-2">
            <div class="card">
              <div class="card-body text-start">
                {{ subject }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomeView',
  data() {
    return {
      students: [],
      subjects: [],
      frontendNode: 'Unknown',
      loading: false
    };
  },
  async created() {
    await this.identifyFrontendNode();
  },
  methods: {
    async identifyFrontendNode() {
      try {
        // First try to get from environment variable (set at build time)
        if (process.env.VUE_APP_NODE_ID) {
          this.frontendNode = process.env.VUE_APP_NODE_ID;
          return;
        }

        // Fallback to checking the server header
        const response = await fetch(window.location.origin, {
          method: 'HEAD',
          cache: 'no-store'
        });
        
        const nodeId = response.headers.get('X-Node-ID');
        
        // Map container hostnames to display names
        const nodeNames = {
          'frontend1': 'Frontend1',
          'frontend2': 'Frontend2',
          'frontend3': 'Frontend3'
        };
        
        this.frontendNode = nodeNames[nodeId] || nodeId || 'Unknown';
      } catch (error) {
        console.error('Error identifying frontend node:', error);
        this.frontendNode = 'Unknown';
      }
    },
    async fetchStudents() {
      this.loading = true;
      this.subjects = [];
      try {
        const response = await fetch('/api/students/');
        if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`);
        const data = await response.json();
        this.students = data.data || data;
      } catch (error) {
        console.error('Fetch students error:', error);
      } finally {
        this.loading = false;
      }
    },
    async fetchSubjects() {
      this.loading = true;
      this.students = [];
      try {
        const response = await fetch('/api/subjects/');
        const result = await response.json();
        this.subjects = result.data || {};
      } catch (error) {
        console.error('Subjects fetch error:', error);
      } finally {
        this.loading = false;
      }
    }
  }
};
</script>

<style scoped>
.home {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 30px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: #333;
  text-align: center;
}

.title {
  font-size: 2.4rem;
  margin-bottom: 10px;
  text-align: center;
}

.subtitle {
  text-align: center;
  color: #666;
  margin-bottom: 30px;
}

.node {
  font-weight: bold;
  color: #2c3e50;
}

.buttons {
  text-align: center;
  margin-bottom: 30px;
}

button {
  padding: 12px 20px;
  margin: 0 10px;
  background-color: #3498db;
  color: white;
  font-size: 1rem;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #2980b9;
}

.section {
  margin-bottom: 40px;
}

.section, .year-section,
.card-list {
  width: 100%;
  max-width: 600px;
}

.section-title {
  font-size: 1.6rem;
  margin-bottom: 20px;
  color: #2c3e50;
}

.year-section {
  margin-bottom: 25px;
}

.year-heading {
  font-size: 1.3rem;
  color: #34495e;
  margin-bottom: 10px;
}

.card-list {
  list-style: none;
  padding: 0;
}

.card {
  background: #f9f9f9;
  border: 1px solid #e1e1e1;
  border-radius: 8px;
  padding: 15px 20px;
  margin-bottom: 10px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.05);
  transition: background 0.2s ease;
  text-align: left;
}

.card:hover {
  background: #f1faff;
}

.loading {
  text-align: center;
  font-size: 1.1rem;
  color: #888;
}

.no-data {
  text-align: center;
  font-size: 1.1rem;
  color: #c0392b;
}
</style>