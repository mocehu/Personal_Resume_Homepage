<template>
    <div class="projects">
        <h1 class="page-title">我的项目</h1>
        <div class="projects-grid">
            <div v-for="project in projects" :key="project.id" class="project-card card">

                <img :src="project.image" class="project-image">

                <!-- <div class="project-image" :style="{ backgroundImage: `url(${project.image})` }"></div> -->
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
import { ref } from 'vue';
import { portfolioProjects } from '../data/personalData';
import Image1 from '../assets/projects/1.png'
import Image2 from '../assets/projects/2.png'
import Image3 from '../assets/projects/3.png'

const projects = ref(portfolioProjects.map(project => {
    // 根据项目ID匹配对应的图片
    let image;
    switch(project.id) {
        case 1:
            image = Image1;
            break;
        case 2:
            image = Image2;
            break;
        case 3:
            image = Image3;
            break;
        default:
            image = '';
    }
    return { ...project, image };
}));
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