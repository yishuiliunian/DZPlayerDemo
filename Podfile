# Uncomment the next line to define a global platform for your project
# platform :ios, '9.0'

require 'pathname'
REPO_ROOT_NAME = '.repo'

def FindRepoRoot(path)
  pwd = Pathname(path)
  if pwd.to_path == '.repo'
    return pwd
  end
  aimPath = nil
  pwd.entries.each { |x|
    if x.to_path == REPO_ROOT_NAME
      aimPath = pwd.join(x)
      break
    end
  }
  if aimPath != nil
    return aimPath
  else
    parentDir = pwd.dirname
    if parentDir.to_path == REPO_ROOT_NAME
      return nil
    end
    return FindRepoRoot(parentDir)
  end
end


REPO_ROOT = FindRepoRoot(Pathname.getwd).realpath


def FindAllSubPodspec(path)
  podspecs = []
  path.entries.each { |entry|
    if (entry.extname == '.podspec' || entry.extname=='.podspec.json')
        podspecs.push(path.join(entry))
    end
  }
  if podspecs.count != 0
    return podspecs
  else
    path.entries.each {  |entry|
      next if entry.to_path == "."
      next if entry.to_path == ".."
      if File.directory?(path.join(entry))
        subpath = path.join(entry)
        ps = FindAllSubPodspec(subpath)
        podspecs = podspecs + ps
      else
      end
    }
  end
  return podspecs
end

DEBUG_PODS = [
]

def PodJoinAllPodspec
  podspecs = FindAllSubPodspec(REPO_ROOT.dirname)
  podspecs.each { |itor|
    name = File.basename(itor,".*")
    path = itor.relative_path_from(Pathname.pwd).dirname
    puts("pod name #{name} path #{path}")
    if not  name.include? "TestCase"
      if DEBUG_PODS.include? name
        pod(name, :path=>path, :configurations => ['Debug'])
      else
        pod(name, :path=>path)
      end
    end
  }
end

target 'DZPlayerDemo' do
  # Uncomment the next line if you're using Swift or would like to use dynamic frameworks
  # use_frameworks!

  # Pods for DZPlayerDemo
  PodJoinAllPodspec()

end
