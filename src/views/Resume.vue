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
                            <span class="info-value">{{ personalInfo.name }}</span>
                        </div>
                    </div>
                    <div class="info-item card">
                        <i class="fas fa-briefcase info-icon"></i>
                        <div>
                            <span class="info-label">职位</span>
                            <span class="info-value">{{ personalInfo.position }}</span>
                        </div>
                    </div>
                    <div class="info-item card">
                        <i class="fa-brands fa-weixin info-icon"></i>
                        <div>
                            <span class="info-label">微信</span>
                            <span class="info-value clickable" @click="showQrcodeModal('wechat')">点击查看二维码</span>
                        </div>
                    </div>
                    <div class="info-item card">
                        <i class="fas fa-envelope info-icon"></i>
                        <div>
                            <span class="info-label">邮箱</span>
                            <span class="info-value clickable" @click="copyToClipboard(personalInfo.contact.email)">{{
            personalInfo.contact.email }}</span>
                        </div>
                    </div>
                    <div class="info-item card">
                        <i class="fab fa-github info-icon"></i>
                        <div>
                            <span class="info-label">Gitee</span>
                            <a class="info-value link" :href="personalInfo.contact.gitee" target="_blank">{{
            personalInfo.contact.giteeUsername }}</a>
                        </div>
                    </div>
                    <div class="info-item card">
                        <i class="fab fa-telegram info-icon"></i>
                        <div>
                            <span class="info-label">Telegram</span>
                            <span class="info-value clickable" @click="showQrcodeModal('telegram')">点击查看二维码</span>
                        </div>
                    </div>
                    <div class="info-item card">
                        <i class="fas fa-map-marker-alt info-icon"></i>
                        <div>
                            <span class="info-label">地点</span>
                            <span class="info-value">{{ personalInfo.location }}</span>
                        </div>
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

            <!-- 教育背景 -->
            <section class="section" id="education">
                <h2 class="section-title">教育背景</h2>
                <div class="education">
                    <div v-for="edu in education" :key="edu.id" class="education-item card">
                        <h3 class="school-name">{{ edu.school }}</h3>
                        <div class="education-header">
                            <p class="degree">{{ edu.major }} | {{ edu.degree }}</p>
                            <span class="time-period">{{ edu.period }}</span>
                        </div>
                        <ul class="edu-responsibilities">
                            <li v-for="(item, index) in edu.responsibilities" :key="index">{{ item }}</li>
                        </ul>
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
                        <div class="other-skills-content" v-for="(skills, title) in otherSkills" :key="skill">
                            <h4 class="skill-title">{{ title }}</h4>
                            <ul class="skill-list">
                                <li v-for="skill in skills" :key="skill">{{ skill }}</li>
                            </ul>
                        </div>
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
                    <!-- <i class="fas fa-heart footer-icon"></i> -->
                    <p class="footer-text">没有更多了哟，感谢您的耐心观看❤️</p>
                </div>
            </footer>
        </div>
        <QrcodeModal v-model:show="showQrcode" :title="qrcodeTitle" :qrcode-url="qrcodeUrl" />
    </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import QrcodeModal from '../components/QrcodeModal.vue';
import telegramQrcode from '../assets/resume/telegram_qrcode.png';
import wechatQrcode from '../assets/resume/wechat_qrcode.png';
import { personalInfo, education, workExperience, resumeProjects, skillLevels, skillCategories, otherSkills, sections } from '../data/personalData';


const showQrcode = ref(false);
const qrcodeTitle = ref('');
const qrcodeUrl = ref('');

const showQrcodeModal = (type) => {
    qrcodeTitle.value = type === 'wechat' ? '微信二维码' : 'Telegram二维码';
    qrcodeUrl.value = type === 'wechat' ? wechatQrcode : telegramQrcode;
    showQrcode.value = true;
};

const projects = ref(resumeProjects);

const getSkillLevel = (skill) => {
    return skillLevels[skill] || 70;  // 默认值70%
};

const copyToClipboard = async (text) => {
    try {
        await navigator.clipboard.writeText(text);
        alert('已复制到剪贴板');
    } catch (err) {
        console.error('复制失败:', err);
    }
};

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
    margin-top: 1.5rem;
    margin-bottom: 0.5rem;
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

.edu-responsibilities,
.project-responsibilities {
    color: #64748b;
    padding-left: 1.25rem;
    margin: 0;
}

.edu-responsibilities li,
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
    margin-top: 0.5rem;
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
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: flex-start;
}

.other-skills-content {
    margin: 5px;
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
    color: var(--primary-dark);
    font-weight: bold;
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