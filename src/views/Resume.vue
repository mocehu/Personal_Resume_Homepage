<template>
    <div class="resume">
        <h1 class="page-title">个人简历</h1>
        <!-- 目录导航 -->
        <nav class="toc card" :class="{ 'show': showToc }">
            <h3 class="toc-title">目录</h3>
            <ul class="toc-list">
                <li v-for="section in sections" :key="section.id">
                    <a :href="`#${section.id}`" @click="scrollToSection(section.id)">{{ section.title }}</a>
                </li>
            </ul>
        </nav>
        <div class="resume-content">
            <!-- 个人信息 -->
            <section class="section" id="personal-info">
                <h2 class="section-title">个人信息</h2>
                <div class="info-grid">
                    <div class="info-item card">
                        <i class="fas fa-user info-icon"></i>
                        <div>
                            <span class="info-label">姓名</span>
                            <span class="info-value">某某</span>
                        </div>
                    </div>
                    <div class="info-item card">
                        <i class="fas fa-briefcase info-icon"></i>
                        <div>
                            <span class="info-label">职位</span>
                            <span class="info-value">前端开发工程师</span>
                        </div>
                    </div>
                    <div class="info-item card">
                        <i class="fas fa-phone info-icon"></i>
                        <div>
                            <span class="info-label">电话</span>
                            <span class="info-value clickable"
                                @click="copyToClipboard('13800138000')">138-0013-8000</span>
                        </div>
                    </div>
                    <div class="info-item card">
                        <i class="fas fa-envelope info-icon"></i>
                        <div>
                            <span class="info-label">邮箱</span>
                            <span class="info-value clickable"
                                @click="copyToClipboard('example@email.com')">example@email.com</span>
                        </div>
                    </div>
                    <div class="info-item card">
                        <i class="fab fa-github info-icon"></i>
                        <div>
                            <span class="info-label">Github</span>
                            <a class="info-value link" href="https://github.com/username" target="_blank">@username</a>
                        </div>
                    </div>
                    <div class="info-item card">
                        <i class="fab fa-linkedin info-icon"></i>
                        <div>
                            <span class="info-label">LinkedIn</span>
                            <a class="info-value link" href="https://linkedin.com/in/username"
                                target="_blank">@username</a>
                        </div>
                    </div>
                    <div class="info-item card">
                        <i class="fas fa-map-marker-alt info-icon"></i>
                        <div>
                            <span class="info-label">地点</span>
                            <span class="info-value">北京</span>
                        </div>
                    </div>
                </div>
            </section>

            <!-- 教育背景 -->
            <section class="section" id="education">
                <h2 class="section-title">教育背景</h2>
                <div class="education">
                    <div v-for="edu in education" :key="edu.id" class="education-item card">
                        <div class="education-header">
                            <h3 class="school-name">{{ edu.school }}</h3>
                            <span class="time-period">{{ edu.period }}</span>
                        </div>
                        <p class="degree">{{ edu.degree }}</p>
                        <p class="major">{{ edu.major }}</p>
                    </div>
                </div>
            </section>

            <!-- 技能特长 -->
            <section class="section" id="skills">
                <h2 class="section-title">技能特长</h2>
                <div class="skills-content">
                    <div class="skills-grid">
                        <div v-for="(skills, category) in skillCategories" :key="category" class="skill-category">
                            <h3 class="category-title">{{ category }}</h3>
                            <div class="skill-tags">
                                <div v-for="skill in skills" :key="skill" class="skill-item card">
                                    <span class="skill-name">{{ skill }}</span>
                                    <div class="skill-progress">
                                        <div class="progress-bar" :style="{ width: getSkillLevel(skill) + '%' }"></div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="other-skills card">
                        <ul class="skill-list">
                            <li v-for="skill in otherSkills" :key="skill">{{ skill }}</li>
                        </ul>
                    </div>
                </div>
            </section>

            <!-- 工作经历 -->
            <section class="section" id="experience">
                <h2 class="section-title">工作经历</h2>
                <div class="timeline">
                    <div v-for="job in workExperience" :key="job.id" class="timeline-item card">
                        <div class="timeline-header">
                            <h3 class="company-name">{{ job.company }}</h3>
                            <span class="time-period">{{ job.period }}</span>
                        </div>
                        <p class="job-title">{{ job.title }}</p>
                        <ul class="job-responsibilities">
                            <li v-for="(item, index) in job.responsibilities" :key="index">{{ item }}</li>
                        </ul>
                    </div>
                </div>
            </section>



            <!-- 项目经历 -->
            <section class="section" id="projects">
                <h2 class="section-title">项目经历</h2>
                <div class="projects">
                    <div v-for="project in projects" :key="project.id" class="card">
                        <div class="project-header">
                            <h3 class="project-title">{{ project.title }}</h3>
                            <span class="time-period">{{ project.period }}</span>
                        </div>
                        <p class="project-description">{{ project.description }}</p>
                        <div class="project-details">
                            <ul class="project-responsibilities">
                                <li v-for="(item, index) in project.responsibilities" :key="index">{{ item }}</li>
                            </ul>
                            <div class="project-tech">
                                <span v-for="tech in project.technologies" :key="tech" class="tag">{{ tech }}</span>
                            </div>
                        </div>
                    </div>
                </div>
            </section>
            <!-- 页脚 -->
            <footer class="resume-footer card">
                <div class="footer-content">
                    <i class="fas fa-heart footer-icon"></i>
                    <p class="footer-text">感谢您的观看</p>
                </div>
            </footer>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const projects = ref([
    {
        id: 1,
        title: '企业级电商平台',
        period: '2022年3月 - 2022年12月',
        description: '基于Vue.js和Node.js的全栈电商解决方案',
        responsibilities: [
            '负责前端架构设计和核心功能开发',
            '实现了高性能的商品搜索和筛选功能',
            '优化了购物车和结算流程的用户体验'
        ],
        technologies: ['Vue.js', 'Node.js', 'MongoDB', 'Redis']
    },
    {
        id: 2,
        title: '数据可视化平台',
        period: '2021年6月 - 2022年2月',
        description: '企业级数据分析和可视化系统',
        responsibilities: [
            '设计和实现了复杂的数据图表展示功能',
            '开发了实时数据更新和监控模块',
            '集成了多种数据源的接入方案'
        ],
        technologies: ['React', 'TypeScript', 'ECharts', 'WebSocket']
    }
]);

const getSkillLevel = (skill) => {
    const levels = {
        'Vue.js': 90,
        'React': 85,
        'TypeScript': 85,
        'Webpack': 80,
        'Tailwind CSS': 85,
        'Node.js': 80,
        'Express': 75,
        'MongoDB': 70,
        'RESTful API': 85,
        'Git': 90,
        'VS Code': 95,
        'Docker': 75,
        'Figma': 80,
        '团队协作': 90,
        '项目管理': 85,
        '问题解决': 90,
        '技术文档编写': 85
    };
    return levels[skill] || 70;  // 默认值70%
};

const skillCategories = ref({
    '核心技能': ['Vue.js', 'React', 'TypeScript', 'Node.js']
});

const otherSkills = ref([
    '精通Socket网络编程和WebSocket实时通信技术',
    '熟练掌握MySQL、MongoDB等数据库的设计与优化',
    '深入理解RESTful API设计原则和微服务架构',
    '熟练使用Docker容器化技术和CI/CD流程',
    '具备良好的代码重构能力和设计模式应用经验'
]);

const workExperience = ref([
    {
        id: 1,
        company: 'XX科技有限公司',
        period: '2021年6月 - 至今',
        title: '高级前端开发工程师',
        responsibilities: [
            '负责公司核心产品的前端架构设计和开发',
            '优化前端性能，提升用户体验',
            '指导初级开发者，组织技术分享会'
        ]
    },
    {
        id: 2,
        company: 'YY互联网公司',
        period: '2019年7月 - 2021年5月',
        title: '前端开发工程师',
        responsibilities: [
            '参与多个项目的前端开发工作',
            '实现响应式设计，确保跨平台兼容性',
            '与后端团队协作，优化接口设计'
        ]
    }
]);

const education = ref([
    {
        id: 1,
        school: 'XX大学',
        period: '2015年9月 - 2019年6月',
        degree: '本科',
        major: '计算机科学与技术'
    }
]);

const copyToClipboard = async (text) => {
    try {
        await navigator.clipboard.writeText(text);
        alert('已复制到剪贴板');
    } catch (err) {
        console.error('复制失败:', err);
    }
};

const sections = ref([
    { id: 'personal-info', title: '个人信息' },
    { id: 'education', title: '教育背景' },
    { id: 'skills', title: '技能特长' },
    { id: 'experience', title: '工作经历' },
    { id: 'projects', title: '项目经历' }
]);

const showToc = ref(true);
const showBackToTop = ref(false);

const scrollToSection = (sectionId) => {
    const element = document.getElementById(sectionId);
    if (element) {
        element.scrollIntoView({ behavior: 'smooth' });
    }
};

const scrollToTop = () => {
    window.scrollTo({
        top: 0,
        behavior: 'smooth'
    });
};

const handleScroll = () => {
    showBackToTop.value = window.scrollY > 300;
};

onMounted(() => {
    window.addEventListener('scroll', handleScroll);
});

onUnmounted(() => {
    window.removeEventListener('scroll', handleScroll);
});
</script>

<style scoped>
.resume {
    height: 100%;
    padding: 2rem;
}

.resume-content {
    max-width: 1000px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    text-align: left;
    padding: 1rem;
}

.section-title {
    color: #2c3e50;
    margin-bottom: 1.5rem;
    padding-bottom: 0.5rem;
    font-size: 1.75rem;
}

.info-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    gap: 1.5rem;
}

.info-item {
    display: flex;
    align-items: center;
    padding: 1.2rem;
    gap: 1rem;
    transition: all 0.3s ease;
    background: white;
    height: 80px;
}

.info-item>div {
    flex: 1;
    min-width: 0;
}

.info-value {
    color: #2c3e50;
    font-weight: 600;
    font-size: 1.1rem;
    display: block;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}

.info-value.clickable {
    cursor: pointer;
    transition: color 0.2s ease;
    max-width: 100%;
}

.info-value.clickable:hover {
    color: var(--primary-light);
}

.info-icon {
    font-size: 1.5rem;
    color: var(--primary-light);
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    background: rgba(59, 130, 246, 0.1);
    border-radius: 10px;
}

.info-label {
    display: block;
    font-weight: 500;
    color: #64748b;
    font-size: 0.875rem;
    margin-bottom: 0.25rem;
}

.info-value {
    color: #2c3e50;
    font-weight: 600;
    font-size: 1.1rem;
}

.skills-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 1.5rem;
}

.category-title {
    font-size: 1.2rem;
    color: #2c3e50;
    margin-bottom: 1rem;
}

.skill-tags {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1rem;
}

.skill-item {
    padding: 1rem;
    transition: transform 0.2s;
}

.skill-item:hover {
    transform: translateY(-2px);
}

.skill-name {
    display: block;
    color: #2c3e50;
    font-weight: 600;
    margin-bottom: 0.5rem;
}

.skill-progress {
    height: 6px;
    background: rgba(59, 130, 246, 0.1);
    border-radius: 3px;
    overflow: hidden;
}

.progress-bar {
    height: 100%;
    background: var(--gradient-primary);
    border-radius: 3px;
    transition: width 1s ease-out;
}

.timeline-item {
    margin-bottom: 2rem;
    padding-left: 1.5rem;
    border-left: 2px solid var(--primary-light);
}

.timeline-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 0.5rem;
}

.company-name {
    font-size: 1.25rem;
    color: #2c3e50;
}

.time-period {
    color: var(--primary-light);
    font-size: 1rem;
    font-weight: 600;
    background: rgba(59, 130, 246, 0.1);
    padding: 0.25rem 0.75rem;
    border-radius: 4px;
    display: inline-block;
}

.job-title {
    color: var(--primary-light);
    font-weight: 600;
    margin-bottom: 0.5rem;
}

.job-responsibilities {
    color: #64748b;
    padding-left: 1.25rem;
}

.job-responsibilities li {
    margin-bottom: 0.25rem;
}

.education-item {
    margin-bottom: 1.5rem;
}

.projects {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
}


.project-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 0.75rem;
}

.project-title {
    font-size: 1.25rem;
    color: #2c3e50;
    font-weight: 600;
}

.project-description {
    color: #64748b;
    margin-bottom: 1rem;
    line-height: 1.5;
}

.project-details {
    display: flex;
    flex-direction: column;
    gap: 1rem;
}

.project-responsibilities {
    color: #64748b;
    padding-left: 1.25rem;
    margin: 0;
}

.project-responsibilities li {
    margin-bottom: 0.5rem;
}

.project-tech {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
}

.education-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 0.5rem;
}

.school-name {
    font-size: 1.25rem;
    color: #2c3e50;
}

.degree,
.major {
    color: #64748b;
    margin-bottom: 0.25rem;
}

.info-value.clickable {
    cursor: pointer;
    transition: color 0.2s ease;
}

.info-value.clickable:hover {
    color: var(--primary-light);
}

.info-value.link {
    color: var(--primary-light);
    text-decoration: none;
    transition: opacity 0.2s ease;
}

.info-value.link:hover {
    opacity: 0.8;
}

@media (max-width: 768px) {
    .resume {
        padding: 1rem;
    }

    .resume-content {
        padding: 1.5rem;
    }

    .section-title {
        font-size: 1.5rem;
    }

    .timeline-header,
    .education-header {
        flex-direction: column;
        align-items: flex-start;
    }

    .time-period {
        margin-top: 0.25rem;
    }
}

.skills-content {
    display: flex;
    flex-direction: column;
    gap: 2rem;
}

.other-skills {
    background: white;
    padding: 1.5rem;
    display: flex;
}

.skill-list {
    list-style-type: none;
    padding: 0;
    margin: 0;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
}

.skill-list li {
    color: #2c3e50;
    padding: 0.5rem 0;
    position: relative;
    padding-left: 1.5rem;
}

.skill-list li::before {
    content: "•";
    color: var(--primary-light);
    position: absolute;
    left: 0;
    font-size: 1.2rem;
}

.resume-footer {
    margin-top: 3rem;
    padding: 2rem;
    /* background: linear-gradient(to right, rgba(59, 130, 246, 0.1), rgba(147, 197, 253, 0.1)); */
}

.footer-content {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1rem;
}

.footer-icon {
    font-size: 1.5rem;
    color: var(--primary-light);
    animation: pulse 1.5s infinite;
}

.footer-text {
    font-size: 1.2rem;
    color: #2c3e50;
    font-weight: 500;
}

@keyframes pulse {
    0% {
        transform: scale(1);
    }
    50% {
        transform: scale(1.2);
    }
    100% {
        transform: scale(1);
    }
}

.toc {
    position: fixed;
    right: 2rem;
    top: 6rem;
    width: fit-content;
    padding: 1.5rem;
    z-index: 100;
}

.toc-title {
    font-size: 1.2rem;
    color: #2c3e50;
    margin-bottom: 1rem;
    padding-bottom: 0.5rem;
}

.toc-list {
    list-style: none;
    padding: 0;
    margin: 0;
}

.toc-list li {
    margin-bottom: 0.5rem;
}

.toc-list a {
    color: #64748b;
    text-decoration: none;
    font-size: 0.9rem;
    transition: color 0.2s ease;
    display: block;
    padding: 0.3rem 0;
}

.toc-list a:hover {
    color: var(--primary-light);
}

.back-to-top {
    position: fixed;
    bottom: 2rem;
    right: 2rem;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: var(--primary-light);
    color: white;
    border: none;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    opacity: 0;
    visibility: hidden;
    transition: all 0.3s ease;
    box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
}

.back-to-top.show {
    opacity: 1;
    visibility: visible;
}

.back-to-top:hover {
    transform: translateY(-2px);
    background: var(--primary-dark);
}

@media (max-width: 1200px) {
    .toc {
        display: none;
    }
}
</style>