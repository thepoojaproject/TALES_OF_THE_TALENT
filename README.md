
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>YouTube Playlist Player</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #00a862 0%, #008c50 100%);
            color: #fff;
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
        }
        
        header {
            text-align: center;
            margin-bottom: 30px;
            padding: 25px;
            background: rgba(0, 0, 0, 0.2);
            border-radius: 15px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .logo {
            text-align: center;
        }
        
        .logo-main {
            font-family: 'Georgia', serif;
            font-size: 3.5rem;
            font-weight: bold;
            color: #fff;
            text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
            letter-spacing: 2px;
            line-height: 1;
        }
        
        .logo-subtitle {
            font-family: 'Georgia', serif;
            font-size: 1.8rem;
            color: #e8f5e9;
            letter-spacing: 4px;
            margin-top: 10px;
            text-transform: uppercase;
        }
        
        .playlist-stats {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 25px;
            flex-wrap: wrap;
        }
        
        .stat {
            background: rgba(255, 255, 255, 0.2);
            padding: 8px 15px;
            border-radius: 20px;
            font-size: 0.9rem;
            border: 1px solid rgba(255, 255, 255, 0.3);
            color: #fff;
        }
        
        .player-section {
            display: flex;
            flex-direction: column;
            gap: 30px;
            margin-bottom: 30px;
        }
        
        @media (min-width: 992px) {
            .player-section {
                flex-direction: row;
            }
        }
        
        .video-container {
            flex: 2;
            background: rgba(0, 0, 0, 0.2);
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .video-wrapper {
            position: relative;
            width: 100%;
            padding-bottom: 56.25%; /* 16:9 aspect ratio */
            height: 0;
        }
        
        .video-wrapper iframe {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            border: none;
        }
        
        .video-info {
            padding: 20px;
        }
        
        .video-title {
            font-size: 1.4rem;
            margin-bottom: 10px;
            color: #fff;
        }
        
        .video-description {
            opacity: 0.9;
            line-height: 1.5;
        }
        
        .playlist-container {
            flex: 1;
            background: rgba(0, 0, 0, 0.2);
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.1);
            display: flex;
            flex-direction: column;
        }
        
        .playlist-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .playlist-header h2 {
            color: #fff;
            font-size: 1.5rem;
        }
        
        .search-box {
            display: flex;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 50px;
            padding: 8px 15px;
            width: 70%;
        }
        
        .search-box input {
            background: transparent;
            border: none;
            color: white;
            width: 100%;
            padding: 5px;
            outline: none;
        }
        
        .search-box input::placeholder {
            color: rgba(255, 255, 255, 0.7);
        }
        
        .search-box i {
            color: rgba(255, 255, 255, 0.7);
            margin-right: 8px;
        }
        
        .playlist-controls {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
            gap: 10px;
        }
        
        button {
            background: rgba(255, 255, 255, 0.9);
            color: #00a862;
            border: none;
            padding: 12px 20px;
            border-radius: 50px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 8px;
            flex: 1;
            justify-content: center;
        }
        
        button:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(255, 255, 255, 0.3);
            background: #fff;
        }
        
        button:active {
            transform: translateY(0);
        }
        
        button:disabled {
            background: rgba(255, 255, 255, 0.3);
            color: rgba(255, 255, 255, 0.7);
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        .video-list {
            max-height: 500px;
            overflow-y: auto;
            border-radius: 10px;
            background: rgba(0, 0, 0, 0.1);
            padding: 10px;
            flex: 1;
        }
        
        .video-item {
            padding: 15px;
            margin-bottom: 10px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.2s ease;
            display: flex;
            border-left: 4px solid transparent;
        }
        
        .video-item:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: translateX(5px);
        }
        
        .video-item.active {
            background: rgba(255, 255, 255, 0.25);
            border-left: 4px solid #fff;
        }
        
        .video-thumb {
            width: 120px;
            height: 68px;
            border-radius: 5px;
            background: rgba(255, 255, 255, 0.9);
            margin-right: 15px;
            flex-shrink: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            color: #00a862;
        }
        
        .video-details {
            flex: 1;
        }
        
        .video-item-title {
            font-weight: 600;
            margin-bottom: 5px;
            font-size: 1rem;
            line-height: 1.3;
        }
        
        .video-item-meta {
            display: flex;
            justify-content: space-between;
            font-size: 0.8rem;
            opacity: 0.8;
        }
        
        footer {
            text-align: center;
            margin-top: 40px;
            padding: 20px;
            font-size: 1.1rem;
            background: rgba(0, 0, 0, 0.2);
            border-radius: 10px;
            color: #fff;
        }
        
        .footer-heart {
            color: #ff6b6b;
            animation: heartbeat 1.5s infinite;
        }
        
        @keyframes heartbeat {
            0% { transform: scale(1); }
            50% { transform: scale(1.2); }
            100% { transform: scale(1); }
        }
        
        /* Scrollbar styling */
        ::-webkit-scrollbar {
            width: 8px;
        }
        
        ::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
        }
        
        ::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.3);
            border-radius: 10px;
        }
        
        ::-webkit-scrollbar-thumb:hover {
            background: rgba(255, 255, 255, 0.5);
        }
        
        /* Responsive adjustments */
        @media (max-width: 991px) {
            .playlist-header {
                flex-direction: column;
                gap: 15px;
            }
            
            .search-box {
                width: 100%;
            }
        }
        
        @media (max-width: 767px) {
            .logo-main {
                font-size: 2.5rem;
            }
            
            .logo-subtitle {
                font-size: 1.3rem;
            }
            
            .playlist-controls {
                flex-direction: column;
            }
            
            .video-item {
                flex-direction: column;
            }
            
            .video-thumb {
                width: 100%;
                height: 120px;
                margin-right: 0;
                margin-bottom: 10px;
            }
        }
        
        .playlist-progress {
            height: 5px;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 5px;
            margin: 15px 0;
            overflow: hidden;
        }
        
        .progress-bar {
            height: 100%;
            background: #fff;
            width: 30%; /* This would be dynamic in a real app */
            border-radius: 5px;
            transition: width 0.3s ease;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <div class="logo">
                <div class="logo-main">TALES OF</div>
                <div class="logo-subtitle">the Talented</div>
            </div>
            <div class="playlist-stats">
                <div class="stat"><i class="fas fa-video"></i> 12 Videos</div>
                <div class="stat"><i class="fas fa-clock"></i> 3h 42m Total</div>
                <div class="stat"><i class="fas fa-eye"></i> 245K Views</div>
            </div>
        </header>
        
        <section class="player-section">
            <div class="video-container">
                <div class="video-wrapper">
                    <!-- YouTube playlist embed -->
                    <iframe width="560" height="315" src="https://www.youtube.com/embed/videoseries?si=Tp4KEq-d0FKp9GlQ&amp;list=PLXYhduqzZYwOf8PTnQj6Q_h_liliEz_Ea" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
                </div>
                <div class="video-info">
                    <h2 class="video-title" id="current-video-title">Introduction to the Playlist</h2>
                    <p class="video-description" id="current-video-description">This is the first video in your playlist. It provides an overview of what viewers can expect from the entire collection.</p>
                </div>
            </div>
            
            <div class="playlist-container">
                <div class="playlist-header">
                    <h2>Playlist Videos</h2>
                    <div class="search-box">
                        <i class="fas fa-search"></i>
                        <input type="text" id="search-input" placeholder="Search videos...">
                    </div>
                </div>
                
                <div class="playlist-progress">
                    <div class="progress-bar"></div>
                </div>
                
                <div class="playlist-controls">
                    <button id="prev-btn" disabled>
                        <i class="fas fa-step-backward"></i>
                        Previous
                    </button>
                    <button id="next-btn">
                        Next
                        <i class="fas fa-step-forward"></i>
                    </button>
                </div>
                
                <div class="video-list" id="video-list">
                    <!-- Video items will be populated by JavaScript -->
                </div>
            </div>
        </section>
        
        <footer>
            <p>Made In <span class="footer-heart">💖</span> By Armeen</p>
        </footer>
    </div>

    <script>
        // Sample playlist data - in a real app, you would fetch this from YouTube API
        const playlistData = [
            {
                id: 1,
                title: "Introduction to the Playlist",
                duration: "10:25",
                views: "45K",
                uploadDate: "2 weeks ago",
                description: "This is the first video in your playlist. It provides an overview of what viewers can expect from the entire collection."
            },
            {
                id: 2,
                title: "Getting Started with the Basics",
                duration: "15:42",
                views: "38K",
                uploadDate: "3 weeks ago",
                description: "Learn the fundamental concepts and get started with the basics of our topic."
            },
            {
                id: 3,
                title: "Advanced Techniques and Methods",
                duration: "22:18",
                views: "32K",
                uploadDate: "1 month ago",
                description: "Dive deeper into advanced techniques that will enhance your skills and knowledge."
            },
            {
                id: 4,
                title: "Tips and Tricks for Efficiency",
                duration: "18:05",
                views: "29K",
                uploadDate: "1 month ago",
                description: "Discover valuable tips and tricks to work more efficiently and effectively."
            },
            {
                id: 5,
                title: "Common Mistakes to Avoid",
                duration: "14:30",
                views: "27K",
                uploadDate: "2 months ago",
                description: "Learn about common pitfalls and mistakes so you can avoid them in your journey."
            },
            {
                id: 6,
                title: "Real World Applications",
                duration: "25:12",
                views: "24K",
                uploadDate: "2 months ago",
                description: "See how these concepts are applied in real-world scenarios and projects."
            },
            {
                id: 7,
                title: "Troubleshooting Guide",
                duration: "16:45",
                views: "22K",
                uploadDate: "3 months ago",
                description: "A comprehensive guide to troubleshooting common issues and problems."
            },
            {
                id: 8,
                title: "Advanced Project Walkthrough",
                duration: "32:10",
                views: "20K",
                uploadDate: "3 months ago",
                description: "Follow along as we build an advanced project from start to finish."
            },
            {
                id: 9,
                title: "Expert Interviews and Insights",
                duration: "28:35",
                views: "18K",
                uploadDate: "4 months ago",
                description: "Gain insights from industry experts and their experiences."
            },
            {
                id: 10,
                title: "Future Trends and Developments",
                duration: "19:20",
                views: "16K",
                uploadDate: "4 months ago",
                description: "Explore upcoming trends and future developments in the field."
            },
            {
                id: 11,
                title: "Resources and Further Learning",
                duration: "12:15",
                views: "14K",
                uploadDate: "5 months ago",
                description: "Discover additional resources to continue your learning journey."
            },
            {
                id: 12,
                title: "Final Thoughts and Conclusion",
                duration: "8:40",
                views: "12K",
                uploadDate: "5 months ago",
                description: "Wrap up the playlist with final thoughts and key takeaways."
            }
        ];

        document.addEventListener('DOMContentLoaded', function() {
            const videoList = document.getElementById('video-list');
            const prevBtn = document.getElementById('prev-btn');
            const nextBtn = document.getElementById('next-btn');
            const searchInput = document.getElementById('search-input');
            const currentVideoTitle = document.getElementById('current-video-title');
            const currentVideoDescription = document.getElementById('current-video-description');
            const progressBar = document.querySelector('.progress-bar');
            
            let currentVideoIndex = 0;
            let filteredVideos = [...playlistData];
            
            // Render video list
            function renderVideoList(videos) {
                videoList.innerHTML = '';
                
                if (videos.length === 0) {
                    videoList.innerHTML = '<div class="video-item" style="text-align: center; justify-content: center;">No videos found</div>';
                    return;
                }
                
                videos.forEach((video, index) => {
                    const videoItem = document.createElement('div');
                    videoItem.className = `video-item ${index === currentVideoIndex ? 'active' : ''}`;
                    videoItem.innerHTML = `
                        <div class="video-thumb">
                            <i class="fas fa-play"></i>
                        </div>
                        <div class="video-details">
                            <div class="video-item-title">${video.title}</div>
                            <div class="video-item-meta">
                                <span><i class="far fa-clock"></i> ${video.duration}</span>
                                <span><i class="far fa-eye"></i> ${video.views}</span>
                                <span><i class="far fa-calendar"></i> ${video.uploadDate}</span>
                            </div>
                        </div>
                    `;
                    
                    videoItem.addEventListener('click', () => {
                        setActiveVideo(index);
                    });
                    
                    videoList.appendChild(videoItem);
                });
                
                updateProgressBar();
            }
            
            // Set active video
            function setActiveVideo(index) {
                currentVideoIndex = index;
                const video = filteredVideos[currentVideoIndex];
                
                // Update current video info
                currentVideoTitle.textContent = video.title;
                currentVideoDescription.textContent = video.description;
                
                // Update button states
                prevBtn.disabled = currentVideoIndex === 0;
                nextBtn.disabled = currentVideoIndex === filteredVideos.length - 1;
                
                // Re-render video list to update active state
                renderVideoList(filteredVideos);
                
                // Scroll to active video
                const activeItem = document.querySelector('.video-item.active');
                if (activeItem) {
                    activeItem.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
                }
            }
            
            // Update progress bar
            function updateProgressBar() {
                const progress = ((currentVideoIndex + 1) / filteredVideos.length) * 100;
                progressBar.style.width = `${progress}%`;
            }
            
            // Filter videos based on search
            function filterVideos() {
                const searchTerm = searchInput.value.toLowerCase();
                filteredVideos = playlistData.filter(video => 
                    video.title.toLowerCase().includes(searchTerm) || 
                    video.description.toLowerCase().includes(searchTerm)
                );
                
                // Reset to first video if current is beyond filtered list
                if (currentVideoIndex >= filteredVideos.length) {
                    currentVideoIndex = 0;
                }
                
                renderVideoList(filteredVideos);
                setActiveVideo(currentVideoIndex);
            }
            
            // Event listeners
            prevBtn.addEventListener('click', function() {
                if (currentVideoIndex > 0) {
                    setActiveVideo(currentVideoIndex - 1);
                }
            });
            
            nextBtn.addEventListener('click', function() {
                if (currentVideoIndex < filteredVideos.length - 1) {
                    setActiveVideo(currentVideoIndex + 1);
                }
            });
            
            searchInput.addEventListener('input', filterVideos);
            
            // Initialize
            renderVideoList(playlistData);
            setActiveVideo(0);
        });
    </script>
</body>
</html>
