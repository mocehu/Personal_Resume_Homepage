<template>
    <div class="projects">
        <h1 class="page-title">我的项目</h1>
        <div class="projects-grid">
            <div v-for="project in projects" :key="project.id" class="project-card card">
                <div class="project-image" :style="{ backgroundImage: `url(${project.image})` }"></div>
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
                            Github
                        </a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';

const projects = ref([
    {
        id: 1,
        title: '项目一',
        description: '这是一个示例项目描述，展示了项目的主要功能和特点。这里可以写比较长的描述，布局会自动适应。',
        image: 'https://via.placeholder.com/400x300',
        tags: ['Vue.js', 'TypeScript', 'Tailwind CSS'],
        demo: 'https://demo.example.com/project1',
        github: 'https://github.com/username/project1'
    },
    {
        id: 2,
        title: '项目二',
        description: '另一个精彩的项目示例，展示了不同的技术栈和实现方案。',
        image: 'https://via.placeholder.com/400x300',
        tags: ['React', 'Node.js', 'MongoDB'],
        demo: 'https://demo.example.com/project2',
        github: 'https://github.com/username/project2'
    },
    {
        id: 3,
        title: '项目二',
        description: '另一个精彩的项目示例，展示了不同的技术栈和实现方案。',
        image: 'https://via.placeholder.com/400x300',
        tags: ['React', 'Node.js', 'MongoDB'],
        demo: 'https://demo.example.com/project2',
        github: 'https://github.com/username/project2'
    }
]);
</script>

<style scoped>
.projects {
    height: 100%;
    padding: 2rem;
}

.projects-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 2rem;
    max-width: 1200px;
    margin: 0 auto;
}

.project-card {
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
    height: 200px;
    background-size: cover;
    background-position: center;
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

@media (max-width: 768px) {
    .projects {
        padding: 1rem;
    }

    .projects-grid {
        grid-template-columns: 1fr;
    }

    .project-buttons {
        flex-direction: column;
    }
}
</style>