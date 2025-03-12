<template>
    <div class="projects">
        <h1 class="page-title">我的项目</h1>
        <div class="projects-grid">
            <div v-for="project in projects" :key="project.id" class="project-card card">
                <img :src="project.image" 
                     :alt="project.title"
                     @error="handleImageError(project)"
                     class="project-image">
                <div class="project-content">
                    <h2 class="project-title">{{ project.title }}</h2>
                    <p class="project-description">{{ project.description }}</p>
                    <div class="project-tags">
                        <span v-for="tag in project.tags" :key="tag" class="tag">{{ tag }}</span>
                    </div>
                    <div class="project-buttons">
                        <a :href="project.demo" target="_blank" class="btn btn-primary" v-if="project.demo">
                            <i class="fas fa-external-link-alt"></i>
                            在线演示
                        </a>
                        <a :href="project.github" target="_blank" class="btn btn-secondary" v-if="project.github">
                            <i class="fab fa-github"></i>
                            开源地址
                        </a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { portfolioProjects } from '../data/personalData';

const defaultImage = 'https://via.placeholder.com/400x300';
const projects = ref([]);

const loadProjectImage = async (projectId) => {
    try {
        const image = await import(`../assets/projects/${projectId}.png`);
        return image.default;
    } catch (error) {
        console.warn(`Failed to load image for project ${projectId}:`, error);
        return defaultImage;
    }
};

const handleImageError = (project) => {
    project.image = defaultImage;
};

onMounted(async () => {
    projects.value = await Promise.all(
        portfolioProjects.map(async (project) => {
            const image = await loadProjectImage(project.id);
            return { ...project, image };
        })
    );
});
</script>

<style scoped>
.projects {
    height: 100%;
    padding: 2rem;
}

.projects-grid {
    columns: 3;
    column-gap: 2rem;
    max-width: 1200px;
    margin: 0 auto;
}

.project-card {
    break-inside: avoid;
    margin-bottom: 2rem;
    height: fit-content;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    background: var(--background-blur);
    border-radius: var(--border-radius);
    transition: var(--transition-base);
}

.project-card:hover {
    transform: translateY(-2px);
    box-shadow: var(--shadow-lg);
}

.project-image {
    background-size: cover;
    background-position: center;
    border-radius: 5px;
    flex-shrink: 0;
}

.project-content {
    display: flex;
    flex-direction: column;
    padding: 0.5rem;
    flex: 1;
    text-align: left;
}

.project-title {
    font-size: 1.5rem;
    color: #2c3e50;
    margin-bottom: 0.4rem;
    line-height: 1.2;
}

.project-description {
    color: #64748b;
    margin-bottom: 0.8rem;
    line-height: 1.5;
    flex-grow: 1;
}

.project-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.4rem;
    margin-bottom: 1rem;
}

.project-buttons {
    display: flex;
    gap: 0.8rem;
    margin-top: auto;
    justify-content: flex-start;
}

.btn-secondary {
    color: var(--primary-light);
    border: 1px solid var(--primary-light);
}

.project-buttons .btn i {
    font-size: 1rem;
}

@media (max-width: 1024px) {
    .projects-grid {
        columns: 2;
    }
}

@media (max-width: 768px) {
    .projects {
        padding: 1rem;
    }

    .projects-grid {
        columns: 1;
    }

    .project-buttons {
        flex-direction: column;
    }
}
</style>