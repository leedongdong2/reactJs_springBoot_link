# reactJs_springBoot_link

vscode로 reactJs와 spring boot 연동하기 

1.일단 vscode와 npm을 이용하기 위한 node.js를 깔아준다<br>
2.vscode 마켓 플레이스에서 Extension Pack for Java 와 spring boot Extension Pack을 설치해준다<br>
3.보기-명령 팔레트-spring initializr-gardle or maven 으로 spring boot 프로젝트를 생성해준다<br>
4.프로젝트가 생성되엇다면 터미널을 열고 src-main으로 이동해서[cd 폴더명] npx create-react-app [프로젝트명]으로 리액트를 설치해준다.[설치가 되었다면 reactjs가 설치된 폴더로 이동하여 npm start로 reactjs가 작동되는지 확인]<br>
5.spring boot[8080]와 reactjs[3000] 포트가 다르기떄문에 cors 에러를 방지하기 위해
react폴더 - package.json에 proxy설정을 해준다.<br>
<img width="236" alt="proxy" src="https://github.com/leedongdong2/reactJs_springBoot_link/assets/77323959/daacf7ca-c439-4528-8bc7-bb7d9854725b"><br>
6.react 세팅을 위해 npm install,npm run-script build, npm run eject를 설치해준다
7.react와 springboot 포트의 패키징을 위해 spring boot프로젝트의 build.gradle에<br>
<br>
def webappDir = "$projectDir/src/main/리액트프로젝트폴더명"

sourceSets {
	main {
		resources {
			srcDirs = ["$webappDir/build", "$projectDir/src/main/resources"]
		}
	}
}

processResources {
	dependsOn "copyWebApp"
}


task copyWebApp(type: Copy) {
    dependsOn "buildReact"
    from "$webappDir/build"
    into "$projectDir/src/main/resources/static"
}


task buildReact(type: Exec) {
	dependsOn "installReact"
	workingDir "$webappDir"
	inputs.dir "$webappDir"
	group = BasePlugin.BUILD_GROUP
	if (System.getProperty('os.name').toLowerCase(Locale.ROOT).contains('windows')) {
		commandLine "npm.cmd", "run-script", "build"
	} else {
		commandLine "npm", "run-script", "build"
	}
}

task installReact(type: Exec) {
	workingDir "$webappDir"
	inputs.dir "$webappDir"
	group = BasePlugin.BUILD_GROUP
	if (System.getProperty('os.name').toLowerCase(Locale.ROOT).contains('windows')) {
		commandLine "npm.cmd", "audit", "fix"
		commandLine 'npm.cmd', 'install'
	} else {
		commandLine "npm", "audit", "fix"
		commandLine 'npm', 'install'
	}
}<br>
을 적어준다.
<br>
8.spring boot서버를 가동하고 8080포트로 들어가서 reactjs 화면이 나온다면 성공

