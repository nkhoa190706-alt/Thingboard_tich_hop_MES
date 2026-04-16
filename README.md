
# Thingboard tích hợp MES

## Giới thiệu
Trang web demo đơn giản sử dụng HTML + JavaScript,
mô phỏng giao diện Web cho hệ thống MES / ThingBoard.

## Công nghệ sử dụng
- HTML
- JavaScript
- GitHub Pages

## Demo
🔗 https://nkhoa190706-alt.github.io/Thingboard_tich_hop_MES/
``
import React, { useState, useEffect } from 'react';
import { 
  LayoutDashboard, 
  Database, 
  ClipboardList, 
  Settings, 
  Play, 
  Pause, 
  AlertCircle, 
  CheckCircle2, 
  Upload,
  Activity,
  Printer
} from 'lucide-react';

// Giả lập ID ứng dụng và cấu hình
const appId = typeof __app_id !== 'undefined' ? __app_id : 'mes-lite-3d';

const App = () => {
  const [activeTab, setActiveTab] = useState('dashboard');
  const [tbConfig, setTbConfig] = useState({
    host: '192.168.1.100', // Thay đổi IP ThingsBoard của bạn ở đây
    token: '' // Token đăng nhập sẽ lấy từ API
  });
  
  // Trạng thái máy in (Dữ liệu này sẽ được đồng bộ từ ThingsBoard)
  const [printers, setPrinters] = useState([
    { id: 'p1', name: 'Máy in 3D số 01', status: 'Sẵn sàng', temp: 25, progress: 0, oee: 85 },
    { id: 'p2', name: 'Máy in 3D số 02', status: 'Đang in', temp: 210, progress: 65, oee: 78 },
  ]);

  // Danh sách đơn hàng/lệnh in (Dữ liệu nội bộ MES)
  const [jobs, setJobs] = useState([
    { id: 'J001', file: 'banh_rang_v1.gcode', customer: 'Anh Tuấn', status: 'Đang in', machine: 'Máy in 3D số 02' },
    { id: 'J002', file: 'vo_hop_du_an.gcode', customer: 'Chị Lan', status: 'Chờ in', machine: '-' },
  ]);

  // Hàm giả lập ra lệnh in sang ThingsBoard qua RPC
  const handleStartPrint = async (jobId, printerId) => {
    console.log(`Đang gửi lệnh RPC sang ThingsBoard cho máy ${printerId} để in lệnh ${jobId}`);
    // Trong thực tế, đây là nơi gọi API: POST /api/plugins/rpc/oneway/{deviceId}
    alert(`Đã gửi lệnh in thành công sang ThingsBoard!`);
  };

  const renderDashboard = () => (
    <div className="space-y-6">
      {/* OEE Summary */}
      <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
        <div className="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
          <div className="flex justify-between items-center mb-2">
            <span className="text-slate-500 font-medium">OEE Trung bình xưởng</span>
            <Activity className="text-blue-500" size={20} />
          </div>
          <div className="text-3xl font-bold text-slate-800">81.5%</div>
          <p className="text-xs text-green-500 mt-2">↑ 2.4% so với hôm qua</p>
        </div>
        <div className="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
          <span className="text-slate-500 font-medium block mb-2">Tổng sản lượng (Pass)</span>
          <div className="text-3xl font-bold text-emerald-600">142</div>
          <p className="text-xs text-slate-400 mt-2">Mục tiêu: 150</p>
        </div>
        <div className="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
          <span className="text-slate-500 font-medium block mb-2">Tỷ lệ lỗi (Q)</span>
          <div className="text-3xl font-bold text-red-500">1.2%</div>
          <p className="text-xs text-slate-400 mt-2">4 lỗi Spaghetti được phát hiện</p>
        </div>
      </div>

      {/* Printer Grid */}
      <h2 className="text-xl font-bold text-slate-800 flex items-center gap-2">
        <Printer size={20} /> Giám sát máy in thời gian thực
      </h2>
      <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
        {printers.map(printer => (
          <div key={printer.id} className="bg-white p-5 rounded-2xl shadow-sm border border-slate-200 flex flex-col gap-4">
            <div className="flex justify-between items-start">
              <div>
                <h3 className="font-bold text-lg">{printer.name}</h3>
                <span className={`text-xs px-2 py-1 rounded-full font-medium ${
                  printer.status === 'Sẵn sàng' ? 'bg-green-100 text-green-600' : 'bg-orange-100 text-orange-600'
                }`}>
                  {printer.status}
                </span>
              </div>
              <div className="text-right">
                <p className="text-xs text-slate-400 uppercase">Hiệu suất OEE</p>
                <p className="text-xl font-bold text-blue-600">{printer.oee}%</p>
              </div>
            </div>
            
            <div className="space-y-2">
              <div className="flex justify-between text-sm">
                <span className="text-slate-500">Tiến độ</span>
                <span className="font-mono font-bold">{printer.progress}%</span>
              </div>
              <div className="w-full bg-slate-100 h-3 rounded-full overflow-hidden">
                <div 
                  className="bg-blue-500 h-full transition-all duration-500" 
                  style={{ width: `${printer.progress}%` }}
                ></div>
              </div>
            </div>

            <div className="flex justify-between items-center text-sm pt-2 border-t border-slate-50">
              <div className="flex gap-4">
                <div>
                  <p className="text-xs text-slate-400">Nhiệt độ</p>
                  <p className="font-bold">{printer.temp}°C</p>
                </div>
                <div>
                  <p className="text-xs text-slate-400">Dòng điện</p>
                  <p className="font-bold text-yellow-600">145W</p>
                </div>
              </div>
              <button className="p-2 hover:bg-slate-100 rounded-lg transition-colors">
                <Settings size={18} className="text-slate-400" />
              </button>
            </div>
          </div>
        ))}
      </div>
    </div>
  );

  const renderJobs = () => (
    <div className="bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden">
      <div className="p-6 border-b border-slate-100 flex justify-between items-center">
        <h2 className="text-xl font-bold text-slate-800">Quản lý Lệnh in & Đơn hàng</h2>
        <button className="bg-blue-600 text-white px-4 py-2 rounded-lg flex items-center gap-2 hover:bg-blue-700 transition-colors">
          <Upload size={18} /> Tải lên G-code mới
        </button>
      </div>
      <table className="w-full text-left">
        <thead className="bg-slate-50 text-slate-500 text-sm uppercase">
          <tr>
            <th className="px-6 py-4 font-semibold">Mã lệnh</th>
            <th className="px-6 py-4 font-semibold">Tên tệp tin</th>
            <th className="px-6 py-4 font-semibold">Khách hàng</th>
            <th className="px-6 py-4 font-semibold">Máy phân công</th>
            <th className="px-6 py-4 font-semibold">Trạng thái</th>
            <th className="px-6 py-4 font-semibold text-right">Thao tác</th>
          </tr>
        </thead>
        <tbody className="divide-y divide-slate-100">
          {jobs.map(job => (
            <tr key={job.id} className="hover:bg-slate-50 transition-colors">
              <td className="px-6 py-4 font-mono font-bold text-blue-600">{job.id}</td>
              <td className="px-6 py-4 text-slate-700">{job.file}</td>
              <td className="px-6 py-4 text-slate-600">{job.customer}</td>
              <td className="px-6 py-4 text-slate-500 italic">{job.machine}</td>
              <td className="px-6 py-4">
                <span className={`flex items-center gap-1 text-sm font-medium ${
                  job.status === 'Đang in' ? 'text-orange-500' : 'text-slate-400'
                }`}>
                  {job.status === 'Đang in' ? <Play size={14} fill="currentColor" /> : <Pause size={14} />}
                  {job.status}
                </span>
              </td>
              <td className="px-6 py-4 text-right">
                {job.status === 'Chờ in' && (
                  <button 
                    onClick={() => handleStartPrint(job.id, 'p1')}
                    className="text-emerald-600 hover:bg-emerald-50 p-2 rounded-lg"
                  >
                    <Play size={20} />
                  </button>
                )}
                <button className="text-slate-400 hover:bg-slate-100 p-2 rounded-lg ml-2">
                  <AlertCircle size={20} />
                </button>
              </td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );

  return (
    <div className="flex min-h-screen bg-slate-50 text-slate-900 font-sans">
      {/* Sidebar Navigation */}
      <aside className="w-64 bg-slate-900 text-white flex flex-col p-4">
        <div className="py-8 px-4">
          <h1 className="text-2xl font-black bg-gradient-to-r from-blue-400 to-emerald-400 bg-clip-text text-transparent">
            MES-LITE 3D
          </h1>
          <p className="text-xs text-slate-500 mt-1 uppercase tracking-widest font-bold">Xưởng sản xuất thông minh</p>
        </div>
        
        <nav className="flex-1 space-y-2">
          <button 
            onClick={() => setActiveTab('dashboard')}
            className={`w-full flex items-center gap-3 px-4 py-3 rounded-xl transition-all ${activeTab === 'dashboard' ? 'bg-blue-600 text-white shadow-lg shadow-blue-900/50' : 'text-slate-400 hover:bg-slate-800'}`}
          >
            <LayoutDashboard size={20} />
            <span className="font-medium">Bảng điều khiển</span>
          </button>
          <button 
            onClick={() => setActiveTab('jobs')}
            className={`w-full flex items-center gap-3 px-4 py-3 rounded-xl transition-all ${activeTab === 'jobs' ? 'bg-blue-600 text-white shadow-lg shadow-blue-900/50' : 'text-slate-400 hover:bg-slate-800'}`}
          >
            <ClipboardList size={20} />
            <span className="font-medium">Lệnh sản xuất</span>
          </button>
          <button 
            onClick={() => setActiveTab('database')}
            className={`w-full flex items-center gap-3 px-4 py-3 rounded-xl transition-all ${activeTab === 'database' ? 'bg-blue-600 text-white shadow-lg shadow-blue-900/50' : 'text-slate-400 hover:bg-slate-800'}`}
          >
            <Database size={20} />
            <span className="font-medium">Kho G-Code</span>
          </button>
        </nav>

        <div className="pt-4 border-t border-slate-800">
          <button 
             onClick={() => setActiveTab('settings')}
             className={`w-full flex items-center gap-3 px-4 py-3 rounded-xl transition-all ${activeTab === 'settings' ? 'bg-slate-700' : 'text-slate-400 hover:bg-slate-800'}`}
          >
            <Settings size={20} />
            <span className="font-medium">Cấu hình hệ thống</span>
          </button>
        </div>
      </aside>

      {/* Main Content Area */}
      <main className="flex-1 overflow-y-auto p-8">
        <header className="flex justify-between items-center mb-8">
          <div>
            <h2 className="text-2xl font-bold text-slate-800">
              {activeTab === 'dashboard' ? 'Giám sát vận hành' : activeTab === 'jobs' ? 'Danh sách lệnh in' : 'Cấu hình hệ thống'}
            </h2>
            <p className="text-slate-500 text-sm">Cập nhật lúc: {new Date().toLocaleTimeString()}</p>
          </div>
          <div className="flex items-center gap-3 bg-white p-2 px-4 rounded-xl border border-slate-200 shadow-sm">
            <div className="w-2 h-2 rounded-full bg-emerald-500 animate-pulse"></div>
            <span className="text-sm font-medium text-slate-600">TB Server: {tbConfig.host} (Connected)</span>
          </div>
        </header>

        {activeTab === 'dashboard' && renderDashboard()}
        {activeTab === 'jobs' && renderJobs()}
        
        {activeTab === 'settings' && (
           <div className="bg-white p-8 rounded-2xl shadow-sm border border-slate-200 max-w-2xl">
              <h2 className="text-xl font-bold mb-6">Kết nối ThingsBoard</h2>
              <div className="space-y-4">
                <div>
                  <label className="block text-sm font-medium text-slate-700 mb-1">Địa chỉ IP Máy chủ (ThingsBoard)</label>
                  <input 
                    type="text" 
                    className="w-full p-3 rounded-lg border border-slate-200 focus:ring-2 focus:ring-blue-500 outline-none"
                    value={tbConfig.host}
                    onChange={(e) => setTbConfig({...tbConfig, host: e.target.value})}
                  />
                </div>
                <div>
                  <label className="block text-sm font-medium text-slate-700 mb-1">Tenant Email</label>
                  <input type="text" className="w-full p-3 rounded-lg border border-slate-200" placeholder="sysadmin@thingsboard.org" />
                </div>
                <button className="bg-blue-600 text-white px-6 py-3 rounded-xl font-bold hover:bg-blue-700 transition-all">
                  Lưu & Kiểm tra kết nối
                </button>
              </div>
           </div>
        )}
      </main>
    </div>
  );
};

export default App;
