package kr.or.ddit.dam.log.controller;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import kr.or.ddit.dam.mem.service.IMemService;
import kr.or.ddit.dam.mem.service.MemServiceImpl;

import java.io.IOException;
import java.io.PrintWriter;

/**
 * Servlet implementation class PassFind
 */
@WebServlet("/PassFind.do")
public class PassFind extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public PassFind() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		String mail = request.getParameter("mem_mail");
		String bir = request.getParameter("mem_bir");
		
		System.out.println("PassFind.do 요청 - mail: [" + mail + "], bir: [" + bir + "]");
		IMemService service = new MemServiceImpl();
		
		String pass = service.PassFind(mail, bir);
		
		System.out.println("조회 결과 : " + (pass == null ? "null" : pass));
		System.out.println("mem_mail: " + mail + ", mem_bir: " + bir);
		
		
		response.setContentType("application/json;charset=UTF-8");
		PrintWriter out = response.getWriter();
		
		if(pass != null) {
		out.write("{\"pass\":\"" + pass + "\"}");
	}else {
			out.print("{}");
		}
			out.flush();
		
		}
	}

